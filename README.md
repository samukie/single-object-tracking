# Single Object Tracking for Autonomous Driving

This project is about evaluating Single Object Tracking by applying the object tracker [SiamMask](https://github.com/foolwood/SiamMask) on the Audi Autonomous Driving Dataset [A2D2](https://www.a2d2.audi/a2d2/en.html) and [KITTI](http://www.cvlibs.net/datasets/kitti/).  
The goal was to test, how applicable SiamMask to the task of tracking individual vehicles in those datasets without explicitly fine-tuning it.  
For details see the whole [report](https://github.com/samuki/single-object-tracking/blob/main/Object_Tracking_Report.pdf)

The GUI in this repository can be used to visualize the application of the tracker on the A2D2, see: 
<p align="center">
  <img src="gifs/gui.gif"  width="400"/>
 </p>

To download and install the repository follow the steps: 
* Download the project 
```
git clone https://github.com/samukie/single-object-tracking.git
cd single-object-tracking
```
* Setup the environment 
```
conda create -n sot python=3.7
conda activate sot
conda install pip opencv pyqt
pip install -r requirements.txt 
bash make.sh 
```
* Download a [subset of the A2D2](https://www.a2d2.audi/a2d2/en/download.html)
* Start the GUI 

```
cd evaluation
python GUI.py --config "../SiamMask/experiments/siammask_sharp/config_davis.json" \
--resume "../SiamMask/experiments/siammask_sharp/SiamMask_DAVIS.pth" --dataset "path/to/a2d2_root" \
--object_lookup "path/to/a2d2_class_list.json"
```

The tracker can be evaluated by using the humanly annotated semantic segmentation and instance segmentation images which were provided by human annotators: 

<p align="center">
<img src="images/20181107133258_camera_frontcenter_000000250.png" width="250" /> <img src="images/20181107133258_instance_frontcenter_000000250.png" width="250" /> <img src="images/20181107133258_label_frontcenter_000000250.png" width="250" />
</p>

To evaluate SiamMask on the A2D2 or KITTI dataset, specify your own [config file](https://github.com/samukie/SingleObjectTracking/tree/main/configs) 
and execute: 

```
python evaluate_dataset.py --eval_config configs/your_config.yaml
```

There are two evaluation modes, the IoU and the end-of-track detection.  
In the A2D2, only a subset of the video frames is annotated with segmentations.
This leads to fail cases, where the tracker switches to other objects, if the gap between the scene displayed in two consecutive frames is too big:  

<p align="center">
<img src="gifs/a2d2.gif" width="350" /> 
  
</p>

This problem does not occur, when applying the tracker on the KITTI dataset: 

<p align="center">
<img src="gifs/kitti.gif" width="500" /> 
</p>
