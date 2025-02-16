# U-NET-Synchrotron_CT-image_segmentation


# CTSegNet

U-CTSegNet is a package for end-to-end 3D segmentation workflow for large X-ray tomographic datasets using 2D U-Net Architecture (UNet).


## The Package
### Installation
To use this code you need to install dependencies by using pip command.
```
pip3 install -r requirements.txt
``` 
U-CTSegNet  
├── Model training  
│   ├── Training.py 
│   ├── U-Net.py
│   └── Loss_functions.py  
├── Test Model  
│   ├── Testing.py    
    └── Visual.py  

### Command-line interface
While executable scripts are provided, it's easy to write your own too. Data formats supported are .tiff sequence and hdf5. Example config files are provided in cfg_files/.  
**TRAIN/TEST:**
Extract training data from arbitrarily sized CT data and ground-truth pairs.  
```
python bin/make_training_dataset.py -c cfg_files/setup_train.cfg
```  
Build and train several Unet-like fCNN architectures for an input image size of your choice.  
```  
python bin/train_fCNN.py -t cfg_files/train.cfg -m cfg_files/models/Unet242.cfg
```  
**SEGMENT:** An end-to-end 3D segmentation workflow that binarizes 2D images extracted from 3D CT data using the fCNN model, then rebuilds the corresponding 3D segmentation map. The hdf5 version is optimized for low RAM usage in very large (>50 GB) datasets.  
```
python bin/run_segmenter.py -c cfg_files/setup_seg.cfg
```
**USE HDF5 FORMAT:** Re-package your CT data into hdf5 format, with methods to determine optimal chunk size. Although optional, using hdf5 format accelerates read/write time while slicing through your datasets. Set -c as chunk size in MB or chunk shape z,y,x.
```
python bin/rw_utils/convert_to_hdf5.py -f my_tiff_folder -o output_file.hdf5 -c 20.0
```

## The Algorithm
### fCNN architecture
<p align="justify">CTSegNet deploys unique Unet-like models trained with focal loss to provide accuracy with reduced number of convolutional layers. The methodology and performance metrics are discussed in our paper<sup>1</sup>.
Here is a sample architecture that you can build using the model_utils sub-module in CTSegNet. We will refer to it as Unet-242 because of the 2-4-2 implementation of pooling layers.</p>

<p align="center">
  <img width="800" src="docs/source/img/Unet242.png">
</p>
