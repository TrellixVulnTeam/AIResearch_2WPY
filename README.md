# AIResearch
Object detection using TensorFlow API

<<<<<<< HEAD
1. describe what the project does
2. its requirements
3. how to use it


=======
To Do List:
1. make a script file to download all necessary packages (python, jupyternotebook, etc.)
2. Follow the tutorial (https://pythonprogramming.net/introduction-use-tensorflow-object-detection-api-tutorial/).
3. ORGANIZE!
>>>>>>> b30cf867b36e2e602095c8d9bc3313008b42e4bf

#to activate tf
source ~/tensorflow/bin/activate

###instruction found in models/research/object_detection/g3doc/installation.md

Step 1. Installation 
#after installation of python 2.7

pip install pillow
pip install lxml
pip install jupyter
pip install matplotlib

pip install tensorflow

git clone https://github.com/tensorflow/models.git

#install protoc if you don't have it already
#to check if you have, run  protoc --version
#https://github.com/google/protobuf/releases
#after installing protobuf, run 
    #sudo ./configure
    #sudo make check
    #sudo make install
    
#run below commands in the directory, models/research 
protoc object_detection/protos/*.proto --python_out=.
export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim

#from models/research run
sudo python setup.py install #this formally installs the object_detection library 
    
Step 2. Check if installation is complete

#go to models/research/object_detection
#open jupyter notebook
#run all object_detection_tutorial.ipynb
    #should get a pic of dogs, people, and kites with boxes
    
Step 3. Set up image files, xxx_label_map.pbtxt, and create_xxx_tf_record.py

#instruction found in models/research/object_detection/g3doc/using_your_own_dataset.md

In the image directory:    
    1. test directory should have a copy of approx 10% of images with annotation data
    2. training directory should have a copy of the rest of data with annotations
    

Make a label map in the directory, models/research/object_detection/data/
    1. label_map should include:

        item {
          id: 1
          name: 'xxx'
        }
    

Make a create_monkey_tf_record.py in the directory, models/research/object_detection/dataset_tools/
Follow the sample code found in models/research/object_detection/g3doc/using_your_own_dataset.md

After creating create_monkey_tf_record.py file, and running it, there should be a train.record and test.record under research/object_detection/data/ file.
Run the file (create_monkey_tf_record.py) by,
python create_monkey_tf_record.py 

Step 4. Using a pre-trained model (transfer learning)

The benefit of transfer learning is that the process is faster with less data required to train.
Need checkpoint and configuration files. 

wget https://raw.githubusercontent.com/tensorflow/models/master/research/object_detection/samples/configs/ssd_mobilenet_v1_pets.config
wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_11_06_2017.tar.gz

Alternatively, checkpoint files can be found here, https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md
And more configuration files can be found here, https://github.com/tensorflow/models/tree/master/research/object_detection/samples/configs

In the configuration file, fix all the path_to_be_cofigured
Class number should be 1
fine_tune_checkpoint: "ssd_mobilenet_v1_coco_11_06_2017/model.ckpt

train_input_reader:
input_path: data/xxx_train.record
label_map_path: data/xxx_label_map.pbtxt

eval_input_reader:
input_path: data/xxx_test.record
label_map_path: data/xxx_label_map.pbtxt

###just for my own reference###
added files are:
    1. test/train.records (data/)
    2. create_xxx_tf.record (dataset_tools/)
    3. xxx_label_map.pbtxt (data/)
    4. ssd_mobilenet_v1_coco_11_06_2017 [directory] (object_detection)
    5. ssd_mobilenet_v1_pets.config [file] (training/)

From models/object_detection, run (train_dir is where all the output will go)
python train.py --logtostderr --train_dir=training/ --pipline_config_path=training/ssd_mobilenet_v1_pets.config
