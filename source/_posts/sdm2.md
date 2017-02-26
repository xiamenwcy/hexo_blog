title: Some improvements about SDM for face alignment (二)
date: 2015-08-15 00:06:00
tags: [机器学习]
mathjax: true
toc: false
categories: [机器学习,人脸识别]#
---
 
训练阶段我们采用了分批处理，可以优化部分内存。

原先我们的代码使用一次性载入数据，然后开始若干次迭代，直至收敛。这样保存image与shape的数据矩阵Data就一直占用内存，但实际上，数据集的image与shape 的作用仅仅是计算$\Delta X$和$\Phi$，两变量的定义参考《 [Supervised Descent Method and its Applications to Face Alignment][1]》，其实也就是init shape与true shape的差值以及init shape的特征向量。计算完了，Data就没用了。于是我们设想分批处理，**每次迭代载入一次数据，用完了就clear**。这样就需要载入多次，可能时间上会浪费的比较多，但还好，可以承受。而且**每次载入数据的时候，也使用分批处理，一次只载入10幅图片的Image/shape，算完了就clear**。
<!--more-->

程序中，我们为了增加样本的数量也使用了flip image操作，你可以选择flip or not flip,可以在setup.m中设置

    options.flipFlag  = 1;   % the flag of flipping。

如下为分批处理的代码：

```matlab
%确定分批的序号
if (isfield(options, 'batch_size'))%批量处理
     batch_fid = cell(ceil(nData / (options.batch_size*basize)), 1);% batch_index为批量处理的索引号， batch_fid为批量处理的原图片序号
    for bb = 1:length(batch_fid)
        batch_fid{bb} = ...
            (bb-1)*options.batch_size+1: ...
            min(bb*options.batch_size,length(imlist));
    end
end

ndata_i=0;%nData的遍历序号

for idata = 1 : length(batch_fid)
   
   for jdata=1:length( batch_fid{idata})
    %% the information of i-th image
    %disp(Data(idata).img);
    disp(['Stage: ' num2str(options.current_cascade) ' - Image: ' num2str(batch_fid{idata}(jdata))]);
   
    Data= load_single_data2 ( imgDir, ptsDir, batch_fid{idata}(jdata),options );

    for kdata=1:basize
        
        ndata_i=ndata_i+1;
        
        img   = Data.img_gray;
        shape = Data.shape_gt;
        %% fipping data,左右翻转图像，用于产生更多的训练数据
        if kdata==2
            
            if size(img,3) > 1
                img_gray   = fliplr(rgb2gray(uint8(img)));
            else
                img_gray   = fliplr(img);
            end

            clear img;
            img = img_gray;
            clear img_gray;

            shape = flipshape(shape);
            shape(:,1) = size(img,2) - shape(:, 1);


            if 0
                figure(1); imshow(img); hold on;
                draw_shape(shape(:,1),...
                    shape(:,2),'y');
                hold off;
                pause;
            end
        end
    
    %% if the first cascade
    if ( current_cascade == 1 )
        
        %% if detect face using viola opencv
        %boxes = detect_face( img , options );
        
        %% if using ground-truth
        bbox = [];
        
        %% predict the face box
        if isempty(bbox)
            %% if using ground-truth
            bbox = getbbox(shape);
        end
        
        %% randomize n positions for initial shapes
        [rbbox] = random_init_position( ...
            bbox, DataVariation, n_init_randoms,options );%获得n个随机的真实人脸框，模拟人脸识别detector
        
        %% iterations of n initial points
        for ir = 1 : n_init_randoms
            
            %% get random positions and inital shape indexs
            cbbox  = rbbox(ir,:);
            
            init_shape = resetshape(cbbox, MeanShape2);%将平均人脸（而不是初始化人脸）对齐到每个算出的真实人脸框上
 
            if 0
                figure(1); imshow(img); hold on;
                rectangle('Position',  cbbox, 'EdgeColor', 'y');
                draw_shape(init_shape(:,1),...
                    init_shape(:,2),'y');
                hold on;
                rectangle('Position',  bbox, 'EdgeColor', 'r');
                draw_shape(shape(:,1),...
                    shape(:,2),'r');
                hold off;
                 pause;
            end           
            
            %% scale coarse to fine %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
            cropIm_scale = imresize(img,current_scale);
            init_shape = init_shape * current_scale;
            true_shape    = shape      * current_scale;
            
            if 0
                figure(1); imshow(cropIm_scale); hold on;
                draw_shape(init_shape(:,1), init_shape(:,2),'g');
                draw_shape(true_shape(:,1), true_shape(:,2),'r');
                hold off;
               % pause;
            end
            
            storage_bbox((ndata_i-1)*n_init_randoms+ir,:) = ...
                getbbox(init_shape);
            
            %% compute the descriptors and delta_shape %%%%%%%%%%%%%%%%%%%%
            
            % storing the initial shape
            storage_init_shape((ndata_i-1)*n_init_randoms+ir,:) = ...
                shape_2_vec(init_shape);
            
            % storing the the descriptors
            tmp = local_descriptors( cropIm_scale, ...
                init_shape,...
                desc_size, desc_bins, options );
            
            storage_init_desc((ndata_i-1)*n_init_randoms+ir,:) = tmp(:);
            
            % storing delta shape
            tmp_del = init_shape - true_shape;
            shape_residual = bsxfun(@rdivide, tmp_del, ...
                storage_bbox((ndata_i-1)*n_init_randoms+ir,3:4));
            
            storage_del_shape((ndata_i-1)*n_init_randoms+ir,:) = ...
                shape_2_vec(shape_residual);
            
            storage_gt_shape((ndata_i-1)*n_init_randoms+ir,:) = ...
                shape_2_vec(shape);
             
           % storage_image{(ndata_i-1)*n_init_randoms+ir}=cropIm_scale;%补充项
        end
        
    else
        
        % for higher cascaded levels
        for ir = 1 : n_init_randoms
            
            init_shape = vec_2_shape(new_init_shape((ndata_i-1)*n_init_randoms+ir,:)');
            
            %% scale coarse to fine %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
            %% scale coarse to fine %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
           cropIm_scale = imresize(img,current_scale);
            init_shape = init_shape * current_scale;
            true_shape = shape      * current_scale;
            
            %% compute the descriptors and delta_shape %%%%%%%%%%%%%%%%%%%%
            
            if 0
                figure(1); imshow(cropIm_scale); hold on;
                draw_shape(init_shape(:,1), init_shape(:,2),'g');
                draw_shape(true_shape(:,1), true_shape(:,2),'r');
                hold off;
                pause;
            end            
            
            storage_bbox((ndata_i-1)*n_init_randoms+ir,:) = ...
                getbbox(init_shape);
            
            %% compute the descriptors and delta_shape %%%%%%%%%%%%%%%%%%%%
            
            % storing the initial shape
            storage_init_shape((ndata_i-1)*n_init_randoms+ir,:) = ...
                shape_2_vec(init_shape);
            
            % storing the the descriptors
            tmp = local_descriptors( cropIm_scale, ...
                init_shape,...
                desc_size, desc_bins, options );
            storage_init_desc((ndata_i-1)*n_init_randoms+ir,:) = tmp(:);
            
            % storing delta shape
            tmp_del = init_shape - true_shape;
            shape_residual = bsxfun(@rdivide, tmp_del, ...
                storage_bbox((ndata_i-1)*n_init_randoms+ir,3:4));
            
            storage_del_shape((ndata_i-1)*n_init_randoms+ir,:) = ...
                shape_2_vec(shape_residual);
            
            storage_gt_shape((ndata_i-1)*n_init_randoms+ir,:) = ...
                shape_2_vec(shape);
            
            % storage_image{(ndata_i-1)*n_init_randoms+ir}=cropIm_scale;%补充项
        end
     end
    clear img;
    clear cropIm_scale;
    clear shape;
    end
    clear Data;
   end  
end
```













  [1]: http://blog.csdn.net/xiamentingtao/article/details/47306887