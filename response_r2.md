We thank the reviewer for the detailed comments and constructive suggestions. Below are our responses to the questions.
### **R1-Q1. Large memory consumption of our model.** 
We adopt the 4D representation without saving the 4D voxel grid. Please refer the answer in All-Q2 for more details.
### **R2-Q2. The use of SelfRecon in data preparation stage.**
Our approach can still outperforms NeuralBody and AnimatableNeRF with SMPL meshes instead of SelfRecon meshes. Please refer the answer in All-Q1 for details.
### **R2-Q3. The rendered images are mostly blurry.**
It is a challenging task to reconstruct human NeRF representation from only monocular inputs, and our results outperform state-of-art works(NeuralBody and AnimatableNeRF). As stated in L295, we assume some details move consistent with the input human surface sequence. Therefore, blurry occurs in these areas. We believe this might be alleviated by adding a per-frame offset in future work.
### **R2-Q4. $(x,y,z,t)$ representation v.s. Our model.**
The revised version will add more ablation studies about our surface-relative representation. Please see All-Q3 for other ablation studies.
Of course, our representation is much better than $(x,y,z,t)$. As mentioned in Section 3.3, the corresponding points in different frames will be mapped to the same surface-relative representation and thus get the same feature vector. Therefore, our surface-relative representation can integrate inter-frame information. However, the $(x,y,z,t)$ representation does not have this characteristic. This $(x,y,z,t)$ representation equals training NeRF for every single frame with monocular input. We implemented this idea and found that this $(x,y,z,t)$ representation can not generate reasonable novel view synthesis results. We will add more details and results of this experiment in the revised version.
### **R2-Q5. Writing.**
Thank you for pointing out this. The resolutions of input images are 540x540(People-Snapshot), 512x512(ZJUMocap), 512x612(our custom data), and 1000x1000(Human3.6M). We will add them to the implementation details in the revised version. 
As mentioned in L226, for a monocular video(People-Snapshot m3c) of 200 frames, we need about 12 minutes to converge on a single NVIDIA GeForce GTX3090 GPU.