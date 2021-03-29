# yolov5
YOLOv5 Object Detection for Pine Cone Images
* original video link: https://www.youtube.com/watch?v=T0DO1C8uYP8 (in Korean)
* brief instruction using Google Colaborator
  * so we can use free provided GPU (Runtime - Change runtime type - GPU) 
* change directory to content
  * %cd /content/
* clone YOLOv5
  * !git clone https://github.com/ultralytics/yolov5.git
* install requirements
  * %cd /content/yolov5/
  * !pip install -r requirements.txt
* you can download weights 'best.pt' and 'last.pt', and jump to the detect section to use them from here
  * trained with more images and added 'best_ext.pt' and 'last_ext.pt'  
* make a new folder and name it as 'dataset' and upload your dataset
  * uploaded files will be deleted after this session
  * you can upload your dataset to Google Drive, so you don't need to upload everytime
* split dataset into train and validation 
  * only if the dataset hasn't been splitted. otherwise just set train and valid folders in data.yaml file
  * %cd /
  * from glob import glob
  * img_list = glob('/content/dataset/(location of images)/*.jpg')
  * from sklearn.model_selection import train_test_split
  * train, val = train_test_split(img_list, test_size=0.2, random_state=2000)
* save locations of train and val
  * with open('/content/dataset/train.txt', 'w') as f:
    * f.write('\n'.join(train) + 'n')
  * with open('/content/dataset/val.txt', 'w') as f:
    * f.write('\n'.join(val) + 'n')
* change data.yaml to indicate above train and val locations (can simply open the file and edit also)
  * import yaml
  * with open('/content/dataset/data.yaml', 'r') as f:
    * data = yaml.load(f)
  * data['train'] = '/content/dataset/train.txt' (or a train directory)
  * data['val'] = '/content/dataset/val.txt' (or a val directory)
  * with open('/content/dataset/data.yaml', 'w') as f:
    * yaml.dump(data, f)
  * modify nc as 1 since we only want to detect pincones
* training
  * %cd /content/yolov5/
  * !python train.py --img 640 --batch 16 --epochs 50 --data /content/dataset/data.yaml --cfg ./models/yolov5s.yaml --weights yolov5s.pt --name pinecones_yolov5s_results
* detecting
  * %cd /content/yolov5/
  * !python detect.py --weights /content/yolov5/runs/train/pinecones_yolov5s_results/weights/best.pt --img 640 --conf 0.5 --source (location of videos or photos)
