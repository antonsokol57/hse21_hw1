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
fastqc -o fastqc paired1.fastq paired2.fastq mate1.fastq mate2.fastq\
mkdir multiqc\
multiqc -o multiqc fastqc

С помощью программ platanus_trim и platanus_internal_trim подрезаем чтения по качеству и удаляем праймеры:

platanus_trim paired1.fastq paired2.fastq\
platanus_internal_trim mate1.fastq mate2.fastq

удаляем исходные .fastq файлы:

rm paired1.fastq\
rm paired2.fastq\
rm mate1.fastq\
rm mate2.fastq

С помощью программы fastQC и multiQC оцениваем качество подрезанных чтений и получаем по ним общую статистику:

mkdir fastqctrimmed\
fastqc -o fastqc paired1.fastq.trimmed paired2.fastq.trimmed mate1.fastq.int_trimmed mate2.fastq.int_trimmed\
mkdir multiqctrimmed\
multiqc -o multiqctrimmed fastqctrimmed

С помощью программы “platanus assemble” собираем контиги из подрезанных чтений:

platanus assemble -f paired1.fastq.trimmed paired2.fastq.trimme 2> assemble.log

С помощью программы “ platanus scaffold” собираем скаффолды из контигов, а также из подрезанных чтений:

platanus scaffold -o Poil -IP1 paired1.fastq.trimmed paired2.fastq.trimmed -OP2 mate1.fastq.int_trimmed mate2.fastq.int_trimmed 2> scaffold.log

С помощью программы “ platanus gap_close” уменьшаем кол-во гэпов с помощью подрезанных чтений:

platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 paired1.fastq.trimmed paired2.fastq.trimmed  -OP2 mate1.fastq.int_trimmed mate2.fastq.int_trimmed 2> gapclose.log

Удаляем подрезанные  .fastq файлы:

rm mate1.fastq.int_trimmed  mate2.fastq.int_trimmed  paired1.fastq.trimmed  paired2.fastq.trimmed

Скриншоты MultiQC Для исходных чтений:

![image](https://user-images.githubusercontent.com/92381120/138922729-068f371a-f6cf-4941-bbd4-d14db21c0f93.png)
![image](https://user-images.githubusercontent.com/92381120/138923019-ccb9bb7b-0820-4a1e-9687-d27beaff829a.png)
![image](https://user-images.githubusercontent.com/92381120/138923243-5e9b3721-8186-4b4f-a527-d3575d0f4eb2.png)
![image](https://user-images.githubusercontent.com/92381120/138923305-0fbeebd8-aec7-4770-acfd-b067e86a70e7.png)
![image](https://user-images.githubusercontent.com/92381120/138923518-f6eab05d-b9cc-4699-91c3-89174ad76336.png)
![image](https://user-images.githubusercontent.com/92381120/138923589-d82a85fb-94ab-4796-b314-bb8c977238b1.png)
![image](https://user-images.githubusercontent.com/92381120/138923712-0e5af16e-d779-44cd-a4b9-0fe7f17db51a.png)
![image](https://user-images.githubusercontent.com/92381120/138923854-1121e02e-fa17-4b33-92bc-5f41807507b3.png)
![image](https://user-images.githubusercontent.com/92381120/138923903-825f93dd-7811-46fd-82e9-320c28bd5de9.png)


