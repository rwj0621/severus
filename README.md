# severus
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
* 重新建立索引


        samtools index -@ 2 /data/renweijie/data/HCC1395/HCC1395_ont/tumor/tumor.ont.bam
        samtools index -@ 2 /data/renweijie/data/HCC1395/HCC1395_ont/normal/normal.ont.bam
    severus --target-bam /data/renweijie/data/HCC1395/HCC1395_ont/tumor/tumor.ont.bam \
        --out-dir /data/renweijie/Softwares/SV_tools/severus/HCC1395_2022_Somatic_SV_output \
        --vntr-bed /data/renweijie/Softwares/SV_tools/severus/human_GRCh38_no_alt_analysis_set.trf.bed \
        --control-bam /data/renweijie/data/HCC1395/HCC1395_ont/normal/normal.ont.bam \
        -t 4

          
