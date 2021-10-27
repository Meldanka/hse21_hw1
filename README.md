# hse21_hw1
   1. Создаем папку hw1:
      ```
      mkdir hw1
      cd hw1
   2. Создаем ссылки в личной папке:
      ```
      ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
   3. Выбираем случайные чтения с помощью команды seqtk (5 миллионов чтений типа paired-end и 1.5 миллиона чтений типа mate-pairs):
      ```
      
      seqtk sample -s1021 oil_R1.fastq 5000000 > sub1.fastq
      seqtk sample -s1021 oil_R2.fastq 5000000 > sub2.fastq
      seqtk sample -s1021 oilMP_S4_L001_R1_001.fastq 1500000 > mp1.fastq
      seqtk sample -s1021 oilMP_S4_L001_R2_001.fastq 1500000 > mp2.fastq
   4. Oцениваем качество исходных чтений, используя программы fastqc и multiqc, скачаем файлы с помощью Cyberduck:
      ```
      
      mkdir fastqc
      ls *.fastq | xargs -P 4 -tI{} fastqc -o fastqc {}

      mkdir multiqc
      multiqc -o multiqc fastqc
   5. Используем программы platanus_trim и platanus_internal_trim, чтобы подрезать чтения и убрать праймеры:
      ```
      
      platanus_trim sub1.fastq sub2.fastq 
      platanus_internal_trim mp1.fastq mp2.fastq  
   6. Удаляем исходные файлы:
      ```
      
      ls -1 *.fastq | xargs -tI{} rm -r {}
   7. Теперь оцениваем качество подрезанных чтений, используя программы fastqc и multiqc:
      ```
      mkdir trimmed_fastq
      mv -v *trimmed trimmed_fastq/
      mkdir trimmed_fastqc
      ls trimmed_fastq/* | xargs -P 4 -tI{} fastqc -o trimmed_fastqc {}
      mkdir trimmed_multiqc
      multiqc -o trimmed_multiqc trimmed_fastqc
   ДО ПОДРЕЗАНИЙ:   
   <img width="867" alt="Снимок экрана 2021-10-27 в 14 33 06" src="https://user-images.githubusercontent.com/91221560/139058062-f976673d-40c0-4a24-ad52-bd6cda57b944.png">
   <img width="867" alt="Снимок экрана 2021-10-27 в 14 33 31" src="https://user-images.githubusercontent.com/91221560/139058091-2b42f018-194b-4d96-90a5-f1e68412f860.png">
   <img width="867" alt="Снимок экрана 2021-10-27 в 14 34 28" src="https://user-images.githubusercontent.com/91221560/139058742-920980ee-eb44-4fdb-b9de-5142297aa4dc.png">
 
   <img width="947" alt="Снимок экрана 2021-10-27 в 14 44 48" src="https://user-images.githubusercontent.com/91221560/139059887-fb649756-1d14-4ad6-8cea-ca775615305f.png">
 
   <img width="862" alt="Снимок экрана 2021-10-27 в 14 58 41" src="https://user-images.githubusercontent.com/91221560/139061871-aa33fa86-7374-464e-9dd4-ab9238b84da6.png">

   ПОСЛЕ ПОДРЕЗАНИЙ:
   <img width="876" alt="Снимок экрана 2021-10-27 в 14 13 50" src="https://user-images.githubusercontent.com/91221560/139055855-f92bc872-1e6a-419a-884f-06831bbffbe6.png">
   <img width="861" alt="Снимок экрана 2021-10-27 в 14 24 22" src="https://user-images.githubusercontent.com/91221560/139056919-65103896-a720-4c56-81df-07767bcc3e8f.png">

   <img width="861" alt="Снимок экрана 2021-10-27 в 14 25 18" src="https://user-images.githubusercontent.com/91221560/139057124-4fa464cb-317b-440e-9f9d-efea918851d8.png">

   <img width="862" alt="Снимок экрана 2021-10-27 в 14 59 47" src="https://user-images.githubusercontent.com/91221560/139061912-e65684cf-3ff9-49af-bd77-6f5a43dc521b.png">
Видим, что после подрезания содержание адапторов уменьшилось.

8. Соберем контиги из чтений, которые подрезали:
    ```
    time platanus assemble -o Poil -f trimmed_fastq/sub1.fastq.trimmed trimmed_fastq/sub2.fastq.trimmed 2> assemble.log
    
9. Теперь скаффолды:
    ```
    time platanus scaffold -o Poil -c Poil_contig.fa -IP1 trimmed_fastq/sub1.fastq.trimmed  trimmed_fastq/sub2.fastq.trimmed -OP2 trimmed_fastq/mp1.fastq.int_trimmed trimmed_fastq/mp2.fastq.int_trimmed 2> scaffold.log
10. Создадим файл с самым длинным скаффолдом и удалим ненужный файл:
    ```
    echo scaffold1_len3838093_cov232 > _tmp.txt
    seqtk subseq Poil_scaffold.fa.txt _tmp.txt > scaffold1_len3838093_cov232.fna
    rm -r _tmp.txt
    
 11. Используя программу platanus gap_close уменьшаем количество гэпов:
     ```
     time platanus gap_close -o Poil -t 1 -c Poil_scaffold.fa -IP1 trimmed_fastq/sub1.fastq.trimmed trimmed_fastq/sub2.fastq.trimmed -OP2 trimmed_fastq/mp1.fastq.int_trimmed trimmed_fastq/mp2.fastq.int_trimmed 2> gapclose.log

 12. В итоге создаем файл longest.fasta:
     ```
     echo scaffold_cov232 > _tmp.txt
     seqtk subseq Poil_gapClosed.fa _tmp.txt > longest.fasta

    
Ссылка на google colab с кодом:
https://colab.research.google.com/drive/1v-0GeajQmD5fXgNxCCZvaaA37F7TN3Ma#scrollTo=b_C1HqDUqpQL
<img width="1189" alt="Снимок экрана 2021-10-27 в 23 01 45" src="https://user-images.githubusercontent.com/91221560/139138770-2918940c-02b0-49d5-b1f3-fa700360af38.png">
<img width="1189" alt="Снимок экрана 2021-10-27 в 23 02 02" src="https://user-images.githubusercontent.com/91221560/139138798-ed1d4ae0-76b2-4f49-841c-c4883f3241df.png">
<img width="1189" alt="Снимок экрана 2021-10-27 в 23 02 29" src="https://user-images.githubusercontent.com/91221560/139138875-241509e4-a57c-440d-9aa7-ad023c56fb47.png">






 
