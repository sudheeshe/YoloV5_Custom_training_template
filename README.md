
# YoloV5 Custom Training


# PREPARE DATA FOR TRAINING

### Data template
- Train, Val and Test folders should have sub-folder named `images` and `labels` 
- labels should be in .txt format
- Test folder not need labels, since we are using test images for prediction
- Note: Ignore on `labels.cache` file it generated during previous training

![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/data_structure_template.png?raw=true)


### Create .zip file with for training 
  1) train
  2) val
  3) test
  4) data.yaml
  5) YOLO_V5_Custom_Training.ipynb
  6) custom_yolov5s.yaml



# PREPARE PAPERSPACE VM

  
  ### Step - 1
  
  #### Create ssh on MobaXterm
  
  - Open a new terminal in `MobaXterm` Run the following command
  
  ```bash
  ssh-keygen
  ```
  
  ```bash
  - cd /home/sudheeshe/.ssh
  ```
  
  ![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/ssh_keygen.png?raw=true)
  
  - Print out your public key with
  ```bash
  cat ~/.ssh/id_rsa.pub
  ```

 ![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/show_ssh_key.png?raw=true)
 
- copy the above ssh key.

 ### Step - 2
 
- Select `Core virtual servers`

![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/1_.png?raw=true)
  
- Select `create a machine` from that select `ML-in-a-box-Ubuntu 20.4` version
  
![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/2_.png?raw=true)
 
- Select GPU as P4000

![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/3_.png?raw=true)
 
- Add SSH key if ssh was not added previously. If ssh key is already added and we need to rewrite the existing ssh key you can go with steps mentioned later on this file.
  
- Click create button to create the VM

 
### First way to access VM using MobiXterm is  
### Method - 1 Adding SSH Keys
  
  - You can add SSH keys on the SSH Keys page in the console. Specify a name for the key and copy/paste the output from step 2 of key generation. Once you’ve added a key, you can select it during machine creation to automatically add it to new CORE machines.
 
#### step - i
  
![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/ssh_add_1.png?raw=true)
  
#### step - ii
  
![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/ssh_add_2.png?raw=true)

- Yellowline shows previously added ssh keys, with the ssh key name
  
#### step - iii
 
![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/ssh_add_3.png?raw=true)

- paste the copied ssh key from MobaXterm here

### Step - 3

- Start the machine we created
- Once the VM is started we can initialize the connection by using ssh, Click on connet button
  
![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/4_.png?raw=true)
  
- This generates the pop-up with ssh command, which we need to copy and paste it on MobaXterm to initiate the connection
  
![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/5_.png?raw=true)
  
- Now open a new terminal on MobaXterm
  
![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/6_.png?raw=true)  
  
- Paste the ssh commad on this new terminal and press enter to make the connection with paperspace VM

![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/7_.png?raw=true) 
  
- Connection will get established, we can now see the folders in the VM marked in red
 
![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/8_.png?raw=true) 
 
- We will be using this terminal for code execution.

### Method - 2 By password

![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/ssh_password.png?raw=true) 

- Password will be sent to the registered email id
- Go to created VM and click on `connect` and copy the ssh command and past on MobiXterm terminal and press enter. 
- While establishing connection to VM using MobiXterm it will prompt for password.
provide the password over there.

### Choose Method - 2 over Method - 1, Since VM will ask for password even if you create connection through ssh. 


# TRAINING YOLOV5 MODEL 

### Create a working directory with project name `eg: PCB_Defect_Detection` on `Paperspace or Colab`


### Change to working directory 

```bash
cd <path of working folder eg: PCB_Defect_Detection>
```

### Check current working directory 

```bash
pwd
```

### Install Miniconda and create environment

- Here we have used Python 3.7 64bit installer, from below link we can choose any other Linux Python installer

https://docs.conda.io/en/latest/miniconda.html#linux-installers

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-py37_4.12.0-Linux-x86_64.sh

bash Miniconda3-py37_4.12.0-Linux-x86_64.sh

source ~/.bashrc

conda create -n <env_name> python=3.7 -y

conda activate <env_name>
```


### Cloning YOLOv5 repo on working folder eg: PCB_Defect_Detection

```bash
git clone https://github.com/ultralytics/yolov5

cd yolov5

git reset --hard fbe67e465375231474a2ad80a4389efc77ecff99
```

### Install dependencies
```bash
pip install -qr requirements.txt 
```

### Copy the above created zip file to the working Directory and unzip it.

- Now folder structure will look like 

      yolov5/
              1) "yolo files downloaded from git repos"
              2) train
              3) val
              4) test
              5) data.yaml
              6) YOLO_V5_Custom_Training.ipynb
              7) custom_yolov5s
      
    

### Save the custom_yolov5s.yaml 

#### what is `custom_yolov5s.yaml` file
~~~
- In `yolov5` folder we already have `yolov5s.yaml` which was used to train `yolov5 small version` (pretrained yolo).
- In order to train our custom yolo we are editing this `yolov5s.yaml` according to our specification.
- We are only changing the `number of classes (nc:)` in this yaml file an creating a new file called `custom_yolov5s.yaml`
~~~

- Since we have already prepared custom_yolov5s.yaml and which is available in zip file.
- So We are moving the `custom_yolov5s.yaml` to the `models/custom_yolov5s.yaml` location as shown below

![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/custom_yaml.png?raw=true)

### Change the data.yaml file `train and val` locations, `names` name of classes and `nc` number of classes

![alt text](https://github.com/sudheeshe/YoloV5_Custom_training_template/blob/main/imgs/data_yaml.png?raw=true)


### Train Custom YOLOv5 Detector

Here, we are able to pass a number of arguments:

```
img: define input image size
batch: determine batch size
epochs: define the number of training epochs. (Note: often, 3000+ are common here!)
data: set the path to our yaml file
cfg: specify our model configuration
weights: specify a custom path to weights. (Note: you can download weights from the Ultralytics Google Drive folder)
name: result names
nosave: only save the final checkpoint
cache: cache images for faster training
```

- We can change file location of`data.yml` and `custom_yolov5s.yaml` if needed, 
- Since we have placed these files inside `yolov5` folder itself, So we can run the below command as it is. We don't need to change the path.

```bash
cd yolov5

python train.py --img 416 --batch 32 --epochs 100 --data data.yaml --cfg models/custom_yolov5s.yaml --weights '' --name yolov5s_results  --cache


```

### To view tensorboard logs

```bash
tensorboard --logdir runs
```

### Run Inference With Trained Weights
- 2 paths we need to provide 
      
      1) best weight path (best.pt)
      2) test image path
   
- We can provide what is the confidence score (threshold)  using `--conf` argument for prediction. whatever predictions below this will not get generated. Closer to `0 - less threshold` closer to `1 - higher threshold`.


```bash
cd yolov5
python detect.py --weights runs/train/yolov5s_results/weights/best.pt --img 416 --conf 0.5 --source test/images
```

- Prediction images will be available in yolov5/runs/detect
  
### Export Trained Weights for Future Inference
  
```bash
from google.colab import drive
drive.mount('/content/gdrive')
```

```bash
cp /content/yolov5/runs/train/yolov5s_results/weights/best.pt /content/gdrive/MyDrive/Research/SignLanguageDetection
```
  
### Reference blogs

- https://towardsdatascience.com/how-to-train-a-custom-object-detection-model-with-yolo-v5-917e9ce13208
- https://github.com/entbappy/Sign-Language-Generation-From-Video-using-YOLOV5
  
