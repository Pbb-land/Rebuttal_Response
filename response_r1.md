We thank the reviewer for the detailed comments and constructive suggestions. Below are our responses to the questions.
### **R1-Q1. The use of SelfRecon in data preparation stage.**
As stated in L254, we used SelfRecon meshes instead of SMPL in NeuralBody for a fair comparison. Thus this comparison is apple-to-apple. Moreover, we outperform NeuralBody when both use Selfrecon meshes. We will add the data preparation time in the revised version.
As reported in All-Q1, we have done an additional experiment on the ZjuMocap dataset to evaluate our method without SelfRecon meshes. The results show that we outperform NeuralBody and AnimatableNeRF when using SMPL meshes. This performance gain is mainly caused by other modules of our method:
- Surface relative representation for aggregating inter-frame information
- Multi-resolution hash encoding for speed-up training
- Removing the direction $d$ from NeRF's input in monocular cases to eliminate the gap of inputs between training and testing



### **R1-Q2. Comparison to SelRecon(Volumetric rendering v.s. Mesh-based rendering).**
As described in All-Q1, SelfRecon is not the necessary component of our data preparing stage. Though this approach relies on consistent human surface sequence, it is not an ''add-on'' nerf module of SelfRecon.
We think mesh-based and volume rendering have their own advantages: Mesh-bases rendering is simple and fast as a kind of forward rendering. Moreover, the rendering results highly depend on the accuracy of reconstructed geometry, texture, lighting, material, etc. While NeRF-like volume rendering method is essentially a fitting program for the input images, it can generate high-fidelity novel view synthesis results without restructuring explicit 3D representation. Therefore, NeRF-like volume rendering relies less on the accuracy of the reconstructed geometry of the target.
For instance, in our experiments, some mesh sequences obtained from SelfRecon have artifacts that can not be ignored(e.g., a big hole on the top of the human head). However, we can still generate reasonable results via volume rendering. We will add this comparison in the revised version.

### **R1-Q3. Discussion and ablation study of vertex-based representation v.s. Our model.**

Though we use 4D representation, the total number of features that we want to obtain through the hash encoder is much less than $N_{max}^4$ , where $N_{max}$ is the finest resolution of multi-resolution hash encoding. This is because the first three dimensions of our input to the hash encoder are only the coordinates of the vertices on the template mesh's surface $S$. 

Actually, if we regard $S$ as a 2D surface and distance $d$ as an additional dimension, our surface-relative representation can be explained as an irregular 3D voxel grid. And the number of features in this 3D grid is $N_v*N_{max}$ , far less than $N_{max}^4$ , where $N_v$ is the number of vertices on $S$ . Moreover, the 1D discrete features in the vertex-based representation are exactly the features in this 3D grid. Thus, this vertex-based representation can be understood as our method without hash encoding. It can be interpreted that when fixing the first three dimensions $(x,y,z)$ as a coordinate of a vertex $v$ on $S$ of the input, the features obtained from the hash encoder by moving the fourth dimension d are exactly the 1D discrete features anchored to $v$ in the vertex-based representation. Generally speaking, the only difference between these two representations is that the vertex-based representation directly saves the features in the irregular 3D voxel grid, while our method uses the hash encoder to get these features and only saves the hash tables.
We have done a comparison experiment. For each vertex in the template mesh, we anchored 500(≈ $N_max$ ) optimizable feature vectors with dimensionality 32. The vertex-based representation takes at least ten times longer to converge and generate similar results. We trained the vertex-representation for 500 epochs(around 5h) compared to our method(20 epochs , around 12min). The average PSNR of our method on 6test views of our custom data is 25.91, comparable to the vertex-based representation(25.48). Moreover, as expressed in All-Q2, we just saved the parameters in hash tables instead of the generated features. Thus, the vertex-based representation needs more parameters than our model( $N_v$ $\times$ $N_{max}$ $\times$ 32≈7000 $\times$ 500 $\times$ 32>16 $\times$ $2^{19}$ $\times$ 2). We will add more details about this ablation study in the revised version.

### **R1-Q4. Discussion about limitations**

Generally speaking, as shown in All-Q1, the mesh reconstruction step is not necessary. Our method can generate better results than NeuralBody and AnimatableNeRF in the monocular case with accurate SMPL meshes. The blurry might be solved by adding a per-frame offset, and we leave it as future work.
