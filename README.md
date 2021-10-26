# hse21_hw1
Список всех команд, которые были выполнены на сервере:

Создаем символическую ссылку на каждый из исходных файлов:

ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq\
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq\
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq\
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq

Выбираем 5 миллионов чтений типа paired-end и 1.5 миллиона чтений типа mate-pairs с параметром seed 1228:

seqtk sample -s1228 _R1.fastq 5000000 > paired1.fastq\
seqtk sample -s1228 _R2.fastq 5000000 > paired2.fastq\
seqtk sample -s1228 oilMP_S4_L001_R1_001.fastq 1500000 > mate1.fastq\
seqtk sample -s1228 oilMP_S4_L001_R2_001.fastq 1500000 > mate2.fastq

С помощью программы fastQC и multiQC оцениваем качество исходных чтений и получаем по ним общую статистику:

mkdir fastqc\
fastqc -o fastqc paired1.fastq paired2fastq mate1.fastq mate2fastq\
mkdir multiqc\
multiqc -o multiqc fastqc
