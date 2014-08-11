# Expression_of_Emotions_in_20th_Century_Chinese_Books


## 1 ����google ngram���ݼ���Chinese 1gram���֣�����ѹ   
���ص�ַ��https://storage.googleapis.com/books/ngrams/books/datasetsv2.html (http://dev.mysql.com/downloads/)    

## 2 ��װMySQL Community Server 5.6.19    
���ص�ַ�� http://dev.mysql.com/downloads/(http://dev.mysql.com/downloads/)

## 3 ����֪��HowNet������дʱ�    
���ص�ַ��http://www.keenage.com/
������дʱ�cnpositiveemotionwords.txt
������дʱ�cnnegitiveemotionwords.txt

## 3 ��1gram���ݵ������ݿ�:    

### 3.1 �½�������ngram���ݿ�    
cmd> mysql -u root -p    
sql shell> create database ngram;      
sql shell> use ngram     

### 3.2 ������cn1gram     
sql shell> create table cn1gram    
        -> ( ngram text default null,
        ->   year int(11) default null,
        ->   match_count int(11) default null,
        ->   volume_count int(11) default null)
        -> engine = innodb;    
sql shell> \q    

### 3.3 SQL������������    
SQL�������ļ���loaddata.txt         
cmd> mysql -u root -p < "path/loaddata.txt"    

## 4 SQL����������һЩ�������򻯴��������    
������1gram����
cmd> mysql -u root -p < "path/summary.txt" > "path/summaryout.txt"    
���ꡰ�ˡ��͡��ġ����ֵ�Ƶ��
cmd> mysql -u root -p < "path/normal1query.txt" > "path/normal1.txt"  
cmd> mysql -u root -p < "path/normal2query.txt" > "path/normal2.txt"   

## 5 ������������͸�����д�����ֵ�Ƶ��

### 5.1 ��Python����SQL����    
Python���룺querygen.py
���ɵ�SQL�������ļ���querypos.txt��queryneg.txt

### 5.2 SQL������ͳ��ÿ�����дʳ��ֵ�Ƶ��    
cmd> mysql -u root -p < "path/querypos.txt" > "path/pos.txt"   
cmd> mysql -u root -p < "path/queryneg.txt" > "path/neg.txt"   

### 5.3 ��Python��΢������5.2����ļ��ĸ�ʽ     
Python���룺process.py
������ļ���posclean.txt��negclean.txt

## 6 ����д�Ƶ������д�����ݿ�

### 6.1 ������sentiment_neg��sentiment_pos
cmd> mysql -u root -p ngram
sql shell> create table sentiment_neg
        -> ( ngram text default = null,
	->   year int(11) default = null,
	->   match_count int(20) default = null,
	->   volume_count int(20) default = null)
	-> engine = innodb;
sql shell> \q

### 6.2 SQL������������
cmd> mysql -u root -p < "path/loadsentiment.txt"

## 7 ����ÿ�����桢������дʷֱ����Ƶ��
cmd> mysql -u root -p < "path/negsum.txt" > "path/negsentiment.txt"
cmd> mysql -u root -p < "path/possum.txt" > "path/possentiment.txt"


## 8 R�л�ͼ������
analysis.Rmd