# Learning3D: A Modern Library for Deep Learning on 3D Point Clouds Data.

**[Documentation](https://github.com/vinits5/learning3d#documentation) | [Blog](https://medium.com/@vinitsarode5/learning3d-a-modern-library-for-deep-learning-on-3d-point-clouds-data-48adc1fd3e0?sk=0beb59651e5ce980243bcdfbf0859b7a) | [Demo](https://github.com/vinits5/learning3d/blob/master/examples/test_pointnet.py)**

Learning3D is an open-source library that supports the development of deep learning algorithms that deal with 3D data. The Learning3D exposes a set of state of art deep neural networks in python. A modular code has been provided for further development. We welcome contributions from the open-source community.

## Available Computer Vision Algorithms in Learning3D

| Sr. No.       | Tasks         | Algorithms  |
|:-------------:|:----------:|:-----|
| 1 | [Classification](https://github.com/vinits5/learning3d#use-of-classification--segmentation-network) | PointNet, DGCNN, PPFNet |
| 2 | [Segmentation](https://github.com/vinits5/learning3d#use-of-classification--segmentation-network) | PointNet, DGCNN |
| 3 | [Reconstruction](https://github.com/vinits5/learning3d#use-of-point-completion-network) | Point Completion Network (PCN) |
| 4 | [Registration](https://github.com/vinits5/learning3d#use-of-registration-networks) | PointNetLK, PCRNet, DCP, PRNet, RPM-Net |
| 5 | [Flow Estimation](https://github.com/vinits5/learning3d#use-of-flow-estimation-network) | FlowNet3D | 

## Available Pretrained Models
1. PointNet
2. PCN
3. PointNetLK
4. PCRNet
5. DCP
6. PRNet
7. FlowNet3D
8. RPM-Net (clean-trained.pth, noisy-trained.pth, partial-pretrained.pth)

## Available Datasets
1. ModelNet40

## Available Loss Functions
1. Classification Loss (Cross Entropy)
2. Registration Losses (FrobeniusNormLoss, RMSEFeaturesLoss)
3. Distance Losses (Chamfer Distance, Earth Mover's Distance)

## Technical Details
### Supported OS
1. Ubuntu 16.04
2. Ubuntu 18.04
3. Linux Mint

### Requirements
1. CUDA 10.0 or higher
2. Pytorch 1.3 or higher

## How to use this library?
**Important Note: Clone this repository in your project. Please don't add your codes in "learning3d" folder.**

1. All networks are defined in the module "models".
2. All loss functions are defined in the module "losses".
3. Data loaders are pre-defined in data_utils/dataloaders.py file.
4. All pretrained models are provided in learning3d/pretrained folder.

## Documentation
B: Batch Size, N: No. of points and C: Channels.
####  Use of Point Embedding Networks:
> from learning3d.models import PointNet, DGCNN, PPFNet\
> pn = PointNet(emb_dims=1024, input_shape='bnc', use_bn=False)\
> dgcnn = DGCNN(emb_dims=1024, input_shape='bnc')\
> ppf = PPFNet(features=['ppf', 'dxyz', 'xyz'], emb_dims=96, radius='0.3', num_neighbours=64)

| Sr. No. | Variable | Data type | Shape | Choices | Use |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 1. | emb_dims | Integer | Scalar | 1024, 512 | Size of feature vector for the each point|
| 2. | input_shape | String | - | 'bnc', 'bcn' | Shape of input point cloud|
| 3. | output | tensor | BxCxN | - | High dimensional embeddings for each point|
| 4. | features | List of Strings | - | ['ppf', 'dxyz', 'xyz'] | Use of various features |
| 5. | radius | Float | Scalar | 0.3 | Radius of cluster for computing local features |
| 6. | num_neighbours | Integer | Scalar | 64 | Maximum number of points to consider per cluster |

#### Use of Classification / Segmentation Network:
> from learning3d.models import Classifier, PointNet, Segmentation\
> classifier = Classifier(feature_model=PointNet(), num_classes=40)\
> seg = Segmentation(feature_model=PointNet(), num_classes=40)

| Sr. No. | Variable | Data type | Shape | Choices | Use |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 1. | feature_model | Object | - | PointNet / DGCNN | Point cloud embedding network |
| 2. | num_classes | Integer | Scalar | 10, 40 | Number of object categories to be classified |
| 3. | output | tensor | Classification: Bx40, Segmentation: BxNx40 | 10, 40 | Probabilities of each category or each point |

#### Use of Registration Networks:
> from learning3d.models import PointNet, PointNetLK, DCP, iPCRNet, PRNet, PPFNet, RPMNet\
> pnlk = PointNetLK(feature_model=PointNet(), delta=1e-02, xtol=1e-07, p0_zero_mean=True, p1_zero_mean=True, pooling='max')\
> dcp = DCP(feature_model=PointNet(), pointer_='transformer', head='svd')\
> pcrnet = iPCRNet(feature_moodel=PointNet(), pooling='max')\
> rpmnet = RPMNet(feature_model=PPFNet())

| Sr. No. | Variable | Data type | Choices | Use | Algorithm |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 1. | feature_model | Object | PointNet / DGCNN | Point cloud embedding network | PointNetLK | 
| 2. | delta | Float | Scalar | Parameter to calculate approximate jacobian | PointNetLK |
| 3. | xtol | Float | Scalar | Check tolerance to stop iterations | PointNetLK |
| 4. | p0_zero_mean | Boolean | True/False | Subtract mean from template point cloud |  PointNetLK |
| 5. | p1_zero_mean | Boolean | True/False | Subtract mean from source point cloud | PointNetLK |
| 6. | pooling | String | 'max' / 'avg' | Type of pooling used to get global feature vectror | PointNetLK |
| 7. | pointer_ | String | 'transformer' / 'identity' | Choice for Transformer/Attention network | DCP |
| 8. | head | String | 'svd' / 'mlp' | Choice of module to estimate registration params | DCP |

#### Use of Point Completion Network:
> from learning3d.models import PCN\
> pcn = PCN(emb_dims=1024, input_shape='bnc', num_coarse=1024, grid_size=4, detailed_output=True)

| Sr. No. | Variable | Data type | Choices | Use |
|:---:|:---:|:---:|:---:|:---:|
| 1. | emb_dims | Integer | 1024, 512 | Size of feature vector for each point | 
| 2. | input_shape | String | 'bnc' / 'bcn' | Shape of input point cloud |
| 3. | num_coarse | Integer | 1024 | Shape of output point cloud |
| 4. | grid_size | Integer | 4, 8, 16 | Size of grid used to produce detailed output | 
| 5. | detailed_output | Boolean | True / False | Choice for additional module to create detailed output point cloud|

#### Use of Flow Estimation Network:
> from learning3d.models import FlowNet3D\
> flownet = FlowNet3D()

#### Use of Data Loaders:
> from learning3d.data_utils import ModelNet40Data, ClassificationData, RegistrationData, FlowData\
> modelnet40 = ModelNet40Data(train=True, num_points=1024, download=True)\
> classification_data = ClassificationData(data_class=ModelNet40Data())\
> registration_data = RegistrationData(algorithm='PointNetLK', data_class=ModelNet40Data(), partial_source=False, partial_template=False, noise=False)\
> flow_data = FlowData()

| Sr. No. | Variable | Data type | Choices | Use |
|:---:|:---:|:---:|:---:|:---:|
| 1. | train | Boolean | True / False | Split data as train/test set |
| 2. | num_points | Integer | 1024 | Number of points in each point cloud |
| 3. | download | Boolean | True / False | If data not available then download it |
| 4. | data_class | Object | - | Specify which dataset to use |
| 5. | algorithm | String | 'PointNetLK', 'PCRNet', 'DCP', 'iPCRNet' | Algorithm used for registration |
| 6. | partial_source | Boolean | True / False | Create partial source point cloud |
| 7. | partial_template | Boolean | True / False | Create partial template point cloud |
| 8. | noise | Boolean | True / False | Add noise in source point cloud |

#### Use of Loss Functions:
> from learning3d.losses import RMSEFeaturesLoss, FrobeniusNormLoss, ClassificationLoss, EMDLoss, ChamferDistanceLoss\
> rmse = RMSEFeaturesLoss()\
> fn_loss = FrobeniusNormLoss()\
> classification_loss = ClassificationLoss()\
> emd = EMDLoss()\
> cd = ChamferDistanceLoss()

| Sr. No. | Loss Type | Use |
|:---:|:---:|:---:|
| 1. | RMSEFeaturesLoss | Used to find root mean square value between two global feature vectors of point clouds |
| 2. | FrobeniusNormLoss | Used to find frobenius norm between two transfromation matrices |
| 3. | ClassificationLoss | Used to calculate cross-entropy loss | 
| 4. | EMDLoss | Earth Mover's distance between two given point clouds |
| 5. | ChamferDistanceLoss | Chamfer's distance between two given point clouds |

### To run codes from examples:
1. Copy the file from "examples" folder outside of the directory "learning3d"
2. Now, run the file. (ex. python test_pointnet.py)
- Your Directory/Location
	- learning3d
	- test_pointnet.py

### References:
1. [PointNet:](https://arxiv.org/abs/1612.00593) Deep Learning on Point Sets for 3D Classification and Segmentation
2. [Dynamic Graph CNN](https://arxiv.org/abs/1801.07829) for Learning on Point Clouds
3. [PPFNet:](https://arxiv.org/pdf/1802.02669.pdf) Global Context Aware Local Features for Robust 3D Point Matching
4. [PointNetLK:](https://arxiv.org/abs/1903.05711) Robust & Efficient Point Cloud Registration using PointNet
5. [PCRNet:](https://arxiv.org/abs/1908.07906) Point Cloud Registration Network using PointNet Encoding
6. [Deep Closest Point:](https://arxiv.org/abs/1905.03304) Learning Representations for Point Cloud Registration
7. [PRNet:](https://arxiv.org/abs/1910.12240) Self-Supervised Learning for Partial-to-Partial Registration
8. [FlowNet3D:](https://arxiv.org/abs/1806.01411) Learning Scene Flow in 3D Point Clouds
9. [PCN:](https://arxiv.org/pdf/1808.00671.pdf) Point Completion Network
10. [RPM-Net:](https://arxiv.org/pdf/2003.13479.pdf) Robust Point Matching using Learned Features
11. [3D ShapeNets:](https://people.csail.mit.edu/khosla/papers/cvpr2015_wu.pdf) A Deep Representation for Volumetric Shapes