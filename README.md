# Audio-Visual Speech Recognition Dataset
This repository contains the source code to generate a database that can be used to train speech recognition systems from visual information. Text and video alignment are millisecond accurate thanks to the IBM Audio-to-Text engine.
The procedure can be used for any language by merely using videos in the desired language.
The generated output consists of a CSV file with the following fields:

| Annotation        | Description | 
| ------------- |:-------------:|
| Link          | YouTube video link without www.youtube.com/ |
| Text          | Spoken text in the video sample       |
| Confidence    | Average speech recognition score in the range [0, 1]   |
| Start         | Sample start timestamp in seconds                 |
| End           | Sample end timestamp in seconds      |
| Mouth features| Eight 3D mouth features covering the inner lips        |
| Difficulty    | Easy, medium or hard, according to the faces angles        |


# Procedure

## 1. Download the videos and place them in a directory.
You can generate several directories for different categories — e.g. news, blogs, etc.
Create a spreadsheet file whose name matches the category that contains the video link followed by the name of the downloaded video. The first row of this file must contain * Link * in the first column and * Video * in the second column. This file serves to take control of the link of each video
and it is used to generate the database with the link.


## 2. Extract the audio from the videos
To extract the audio from the videos run the script **extract_wav_files.py**
```
python extract_wav_files.py --dir [directory] --cat [category]
```
where:
- *directory* is the path to the main directory of step 1
- *category* is the video category or subdirectory 

For example, if the videos are inside *c:/videos/news* 
```
python extract_wav_files.py --dir c:/videos --cat news
```

## 3. Extract text from the videos
To extract the text from the videos, we use IBM's **text-to-speech** engine.
In order to execute this part, you need to create an accout in [IBM Cloud](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser). 
Then, you need to create a resource of the type **Speech to Text** to obtain the user name and password.
Once you have these credentials, open the file *extract_detailed_text_watson.py** and edit the fields  **IBM_USERNAME** and **IBM_PASSWORD**. Then execute the script as follows:
```
python extract_detailed_text_watson.py --dir [directory] --cat [category]
```

## 4. Extract subvideos
Once you have the subtitles of each video, execute **extract_subvideos.py**
```
python extract_subvideos.py --dir [directory] --cat [category] --vids_log [log-file] --results_dir [results directory] --ann_file [annotations file]
```
where:
- *directory* is the path to the main directory of step 1
- *category* es the category or subdirectory. 
- *log-file* is the name of a text file that will save the names of the videos that have been processed. This file is automatically created if does not exists and it is saved inside the results directory.
- *results directory* output path where the results are saved.
- *annotations file* output file that contains the annotations.

**Publication**  
Audio-visual database for Spanish-based speech recognition systems.   
Córdova Diana, Terven Juan, Romero Alejandro, and Herrera Ana
En revision.

Licence
----

MIT
