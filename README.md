# severus
## 一、conda安装severus
    conda create -n severus_env severus
    conda activate severus_env
    severus --help
## 二、运行severus
    severus --target-bam /data/renweijie/data/HCC1395/HCC1395.GRCh38.bam \
        --out-dir /data/renweijie/Softwares/SV_tools/severus/HCC1395_Somatic_SV_output \
        --control-bam /data/renweijie/data/HCC1395/HCC1395-BL.GRCh38.bam \
        --threads 4
