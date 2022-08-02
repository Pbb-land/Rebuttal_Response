We thank the reviewer for the detailed comments and constructive suggestions. Below are our responses to the questions.
### **R3-Q1. Ablation study on the number of the k-nearest points.**
We have done this ablation study about the number of the nearest surface points to find the best k before. The results show that when k>=3, this factor has little effect on the final result. When k=1, blur occurs since features in the 3D space are discrete(imaging that points in a small ball near a vertex of the current surface share the same feature). Therefore, we choose k=3 in our final pipeline by balancing the computation time and rendering quality. We will add more details about this ablation study in the revised version. For more other ablation studies, please refer All-Q3.

### **R3-Q2. The major technical components that lead to performance gain.**
The role of geometric guidance loss is to guide the model to converge in the direction of the humanoid more quickly during the early stage of training, preventing it from converging to other local optimal results. Moreover, it has no improvement to final quality since the human surface is not always accurate (SMPL meshes case).
The use of hash encoding through surface-relative representation is the primary technical component. As reported in R1-Q3, the ablation study shows that hash encoding can speed up training and increase rendering results. Moreover, we remove the direction d from NeRF's input. Because in monocular cases, the inputs d are generated from the same camera pose in the training stage. When generating novel view synthesis images for a given camera pose, there may be a large gap between input direction d' and training direction d.

### **R3-Q3. Novel pose synthesis and shape editing.**
We have shown some novel pose synthesis results in our experiments (Fig 5, unseen pose part) and videos(from 1:50 to 2:10). Shape editing is interesting and useful for many applications, and we can add this in the revised version.

### **R3-Q4. Monocular in-the-wild videos.(discussion about limitaions)** 
If the SMPL parameters of the human body can be estimated well, our method can be applied to in-the-wild videos. However, this is challenging to achieve under the premise of monocular input. We believe this problem will be solved in the near future as the accuracy and speed of human shape reconstruction methods have already been greatly improved. In the revised version, we will add comparison experiments with Human NeRF on in-the-wild videos.
