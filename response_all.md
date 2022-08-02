We thank the reviewers for their constructive comments. Below are our responses to those common questions.

### **All-Q1. The use of SelfRecon in data preparation stage.**
The use of SelfRecon makes reviewers feel that this paper is unacceptable. Reviewer gbPx believes that our faster radiance field optimization is built upon the cost of heavier mesh reconstruction(SelfRecon) and considers our work an “add-on” nerf module to SelfRecon. Reviewer k8XC feels that it is meaningless to train our representation after training SelfRecon. Actually, the role of SelfRecon is overestimated in our work, and we clarify below.

Selfrecon is not the necessary component of our data preparing stage. We have shown results on multi-view inputs in the supplementary material(Tab.1 and Fig.3 in supplementary material). In this multi-view experiment, we use the SMPL meshes as the rough human surface and can generate similar quality results with NeuralBody and AnimatableNeRF while with much less training time. This acceleration is based on our novel surface-relative representation with hash encoding and our geometry guidance loss during the early stage of training.

The reason why we use SelfRecon for monocular video inputs is that: 

1)The SMPL parameters obtained from monocular videos are not accurate enough(we use video avatar and EasyMocap to do this work). To obtain a more stable human pose estimation, we choose SelfRecon. This is a general problem for human performer reconstruction from monocular videos. Neural Body has the same problem, so we use SelfRecon meshes instead of SMPL in its pipeline for a fair comparison, as mentioned in L254.

2)As mentioned in L275, we assume that some details like the hands and wrinkles of the clothes move consistent with the input human surface sequence. Therefore, the human surface sequence with non-rigid deformation obtained from SelfRecon improves the rendering quality in some details.
Therefore, what we need is a rough geometry surface sequence that tracks more accurately than SMPL obtained from monocular videos. Many other methods can achieve the same effect with much less computation time(e.g., PIFu-based methods + non-rigid registration). As stated in L280, we believe that this problem will be solved as the accuracy and speed of human shape reconstruction methods have already been greatly improved.

We have done an additional experiment with monocular video inputs to evaluate our method with more accurate SMPL meshes. We use multi-view inputs in EasyMocap to obtain more accurate SMPL parameters, and just use one view to train the model. The average PSNR of our method with accurate SMPL meshes on the ZJUMocap datasets is 24.63, comparable to AnimatableNeRF's results(23.48) and our method with SelfRecon meshes(24.85). This result demonstrates that if the SMPL parameters are accurate enough, our method can also achieve high-quality results without SelfRecon meshes in most cases.

### **All-Q2. Large memory consumption of our model.**
Reviewer gbPx and k8XC concern that our model has a large memory consumption as we adopt 4D voxel fields.

Just like NeRF using 5D representation, we use the 4D representation without saving the 4D voxel grid. Therefore, the final model only contains the multi-resolution hash tables, per-frame's appearance code, and networks' parameters. When a single hash table has 2^19 features with dimensionality 2, our model is about 180MB.

### **All-Q3. More Ablation studies about our surface-relative representation.**
We compared our representation with the vertex-based representation, the (x,y,z,t) representation, and evaluated the effect of the number of nearest neighbors on rendering quality.
- **Our model v.s. Vertex-based representation.** 
Reviewer gbPx proposed a vertex-based representation: Storing 1D discrete feature representations for each vertex instead of a grid-based 4D hashtable. So we have a discussion here. This representation can be understood as our method without hash encoding.
Moreover, the experiments show that the vertex-based representation takes at least ten times longer to converge and generate similar results. The average PSNR of our method on 6 test views of our custom data is 25.91, comparable to the vertex-based representation(25.48). For more details, please see R1-Q3.

- **Surface-relative representation v.s. $(x,y,z,t)$ representation.** 
Reviewer k8XC considers that the $(x,y,z,t)$ representation can also adopt the multi-resolution hash encoding to monocular human reconstruction. So we implemented this idea and found that this $(x,y,z,t)$ representation can not generate reasonable novel view synthesis results. Please see R2-Q4 for more details.
- **Ablation study on the number of the k-nearest points.**
 The experiment results show that when k>3, this factor has little effect on the final result. When k=1, blur occurs since features in the 3D space are discrete. Please see R3-Q1 for more details.
