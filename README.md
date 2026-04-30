# [severus](https://github.com/KolmogorovLab/Severus)
## 一、conda安装severus
    conda create -n severus_env severus
    conda activate severus_env
    severus --help
## 二、运行severus
### 1.校验bam文件的MD5
    md5sum /data/renweijie/data/HCC1395/HCC1395_ont/tumor/tumor.ont.bam
    md5sum /data/renweijie/data/HCC1395/HCC1395_ont/normal/normal.ont.bam
### 2.运行severus
    severus --target-bam /data/renweijie/data/HCC1395/HCC1395.GRCh38.bam \
        --out-dir /data/renweijie/Softwares/SV_tools/severus/HCC1395_Somatic_SV_output \
        --vntr-bed /data/renweijie/Softwares/SV_tools/severus/human_GRCh38_no_alt_analysis_set.trf.bed \
        --control-bam /data/renweijie/data/HCC1395/HCC1395-BL.GRCh38.bam \
        -t 4
* 分型文件

        #在本地拉取给的镜像
        docker pull kishwars/pepper_deepvariant:r0.8-gpu
        #将镜像保存
        d:
        cd D:\docker-images
        docker save kishwars/pepper_deepvariant:r0.8-gpu -o pepper_r0.8.tar
        # 上传到服务器
        #在服务器上进行镜像转换
        singularity build pepper_deepvariant_r0.8-gpu.sif docker-archive://pepper_r0.8.tar
        
### 3.用truvari验证结果的准确性
#### 与severus文章提供的结果对比
* 添加VAF字段

  在结构变异（SV）分析中，原始文件往往只记录支持变异的读数，而 VAF 通过计算变异读数与总覆盖度的比例，直观地告诉我们这个变异在样本中出现的频率：它可以帮助我们剔除测序噪声和假阳性（通常表现为极低的 VAF）
  
          conda deactivate
          conda activate Truvari
          python /data/renweijie/Softwares/SV_tools/severus/HCC1395_Somatic_SV_output/preprocess_for_truvari/severus_add_VAF.py
* 过滤 BND（断点）类型的重复记录
  
  结构变异中的“易位”或“复杂重排”通常用 BND (Breakend) 类型表示。在 VCF 标准中，一个完整的 BND 变异通常由两条记录组成（互相指向对方的 MATE），代表断裂点的两端。
  但是，Truvari 在进行变异比对时，无法处理这种成对的重复记录（它只需要一个代表即可）。如果直接输入原始文件，Truvari 可能会报错或导致计数翻倍。
  
          python /data/renweijie/Softwares/SV_tools/severus/HCC1395_Somatic_SV_output/preprocess_for_truvari/severus_bnd_dedup_for_truvari.py
* 两个结果进行比对

        # 进入目录
        cd /data/renweijie/Softwares/SV_tools/severus/HCC1395_Somatic_SV_output/preprocess_for_truvari/truvari_vcfs/
        # 为两个文件建立索引
        tabix -p vcf severus_HIFI_vaf.vcf.gz
        tabix -p vcf severus_somatic_vaf.vcf.gz
        cd /data/renweijie/Softwares/SV_tools/severus/HCC1395_Somatic_SV_output/preprocess_for_truvari/truvari_output
        truvari bench \
        -b /data/renweijie/Softwares/SV_tools/sniffles2/truvari_vcfs/sniffles2_HIFI_vaf.vcf.gz \
        -c /data/renweijie/Softwares/SV_tools/sniffles2/truvari_vcfs/HCC1395_somatic_vaf.vcf.gz \
        --typeignore \
        --dup-to-ins \
        -p 0 \
        -s 30 \
        -S 0 \
        --sizemax 100000000 \
        --passonly \
        -o Sniffles2_truvari_output
### 4.绘图 从SV数量类型进行比较
#### （1）与severus文章提供的结果对比


        conda activate plot_env
        python /data/renweijie/python_plots/Severus_myResults/compare_sv_counts.py
#### (2)与金标准对比

        

  

        
          
