# Body Emotion Detection:

This project aims to detect emotions from a body reading algorithm. The first step is to unserstand key points of the body moviment, using real time camera or videos/images. 


## SetUp

### Get code and pre-build stuff

- Clone this repository:
```
  git clone git@github.com:raphael-luciano/body-feeling-reader.git
```

- For this project we are using Python3.7
```
sudo apt install python3
```

- Make sure you have Pip3 installed
```
sudo apt-get install python3-pip
```

- We are also using virtual env to keep dependencies isolated from global environment
```
sudo pip3 install virtualenv
```

- Build c++ library for post processing, needed for this project (installing the swig first). 
```
sudo apt install swig
cd tf_pose/pafprocess
swig -python -c++ pafprocess.i && python3 setup.py build_ext --inplace
```

- Pre-Install Jetson case, needed for this project
```
sudo apt-get install libllvm-7-ocaml-dev libllvm7 llvm-7 llvm-7-dev llvm-7-doc llvm-7-examples llvm-7-runtime
export LLVM_CONFIG=/usr/bin/llvm-config-7 
```



### Install Dependencies 

*obs: all ```pip3 install``` should be inside the virtual env - ensure the ```(venv)``` appears at the begining of the line*

- Create virutal environment, on the project's root directory:
```
virtualenv venv
```

- Activate the virutal env
```
source venv/bin/activate
```

- Install C extensions for python (Cython)
```
pip3 install --upgrade cython
```

- Install numpy
```
pip3 install numpy
```

- Install other dependemcies of the project
```
pip3 install -r requirements.txt
```

- Install TensorFlow
```
pip3 install -q tensorflow==1.14 (required v1.14, v2+ don't have tensorflow.contrib, which is used here)
pip3 install tensorflow-gpu (maybe you need to specify version as well. I didn't have to)
```


- Install OpenCV
```
pip3 install opencv-python
```

- Download Tensorflow Graph File (* *optional, I guess =)*)
```
cd models/graph/cmu
bash download.sh
```



## Run

### Test Inference

Test the inference feature with a single image (image flag MUST be relative, wihout \~)

```
python run.py --model=mobilenet_thin --resize=432x368 --image=./images/p1.jpg
```


### Run with realtime Webcam

```
python run_webcam.py --model=mobilenet_thin --resize=432x368 --camera=0
```

Apply TensoRT (not working for now, I didn't have time to figure this out yet)

```
python run_webcam.py --model=mobilenet_thin --resize=432x368 --camera=0 --tensorrt=True
```


## Python Usage

This pose estimator provides simple python classes that you can use in your applications.

See [run.py](run.py) or [run_webcam.py](run_webcam.py) as references.

```python
e = TfPoseEstimator(get_graph_path(args.model), target_size=(w, h))
humans = e.inference(image)
image = TfPoseEstimator.draw_humans(image, humans, imgcopy=False)
```

If you installed it as a package,

```python
import tf_pose
coco_style = tf_pose.infer(image_path)
```

## References

Github with original code taken by this project:
https://github.com/ildoonet/tf-pose-estimation

video tutorial (not updated with the above github code/steps):
https://www.youtube.com/watch?v=nUjGLjOmF7o

Other interesting libraries/links (Face, Gender and age classification, Person Identification, Motion Prediction and Emotion Detection):
https://medium.com/@samim/human-pose-detection-51268e95ddc2 



## ROS Support

See : [etcs/ros.md](./etcs/ros.md)

## Training

See : [etcs/training.md](./etcs/training.md)

## References Inside this Project

See : [etcs/reference.md](./etcs/reference.md)

