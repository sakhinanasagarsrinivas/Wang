You're correct. Upon closer examination of the patent, I see that the **edge features \( e_{ij} \)** play a critical role in the methodology described. Let's revise the explanation, ensuring that the claims, equations, and novelty are properly aligned with the patent.

---

### **Technical Problem**  
The technical problem addressed in the patent revolves around the inefficiencies and limitations of existing methods for molecular property prediction, particularly:  
1. **Inadequate Use of Edge Features**: Conventional models, including MPNNs, do not effectively leverage edge features \( e_{ij} \), which are crucial for encoding chemical bond information in molecular graphs.  
2. **Loss of Structural Information**: Existing pooling methods fail to capture both node and edge information hierarchically, leading to suboptimal graph representations.  
3. **High Computational Cost**: Computational overhead in conventional methods limits their real-time applicability in molecular property prediction tasks.

---

### **Technical Advancement**  

The invention introduces a **Hierarchical Layer-Wise Graph Pooling Method** that incorporates **edge features \( e_{ij} \)** into the molecular graph representation. Key advancements include:  

1. **Edge-Conditioned Node Transformation**:  
   - The node features are transformed by incorporating edge features \( e_{ij} \), enhancing the representational capacity of the graph.  
   - This transformation ensures that the chemical bonds (edge attributes) influence the node embeddings directly.  

2. **Hierarchical Propagation and Pooling**:  
   - The system performs **scalar projection** of node attributes, influenced by \( e_{ij} \), to rank nodes for pooling.  
   - Uses a **down-sampling mechanism** to generate coarsened molecular graphs while preserving important graph-level features.  

3. **Graph-Level Representation via Edge Information**:  
   - Edge-aware graph pooling captures both local and global structural properties, resulting in robust graph-level representations for molecular property prediction.  

---

### **Claims Grounded in the Patent**  

Key claims outlined in the patent include:  

1. **Edge-Conditioned Hierarchical Graph Pooling**:  
   - The method includes transforming node features using a **feed-forward layer** that incorporates both node attributes \( h_i \) and edge features \( e_{ij} \).  

2. **Scalar Projection with Edge Awareness**:  
   - Computes scalar projections \( z_i \) for ranking nodes, where \( z_i \) depends on edge-conditioned transformations.  

3. **Iterative Down-Sampling**:  
   - Down-samples molecular graphs into coarsened graphs by retaining nodes based on their projection scores. Adjacency and feature matrices are updated accordingly.  

4. **Graph Representation Vector**:  
   - The coarsened graph is used to compute a **graph-level embedding** that includes the influence of \( e_{ij} \) for molecular property prediction.  

---

### **Equations from the Patent Including Edge Features**  

1. **Node Feature Transformation**:  
   The transformation of node features incorporates edge features \( e_{ij} \):  
   \[
   \mathbf{h}_i^{(l+1)} = \Theta \cdot \mathbf{h}_i^{(l)} + \sum_{j \in \mathcal{N}(i)} f(e_{ij}) \cdot \mathbf{h}_j^{(l)}
   \]  
   where \( f(e_{ij}) \) is a function mapping edge features to weights for neighboring node contributions.

2. **Scalar Projection for Node Ranking**:  
   The scalar projection of node attributes includes edge influence:  
   \[
   z_i = \mathbf{h}_i^{(l)} \cdot \frac{\mathbf{v}}{\|\mathbf{v}\|} + \sum_{j \in \mathcal{N}(i)} g(e_{ij})
   \]  
   where \( g(e_{ij}) \) adjusts the projection based on edge features.

3. **Pooling and Adjacency Matrix Update**:  
   The pooling mechanism retains nodes based on their projection scores, and the adjacency matrix is updated as:  
   \[
   A^{(l+1)} = P^\top A^{(l)} P
   \]  
   where \( P \) includes the node indices selected based on \( z_i \).  

4. **Graph-Level Representation**:  
   The final graph embedding \( \mathbf{G} \) incorporates both node and edge features:  
   \[
   \mathbf{G} = \text{Readout}(F^{(L)}, E^{(L)})
   \]  
   where \( E^{(L)} \) represents the edge attributes of the coarsened graph at layer \( L \).

---

### **Novelty**  

1. **Integration of Edge Features \( e_{ij} \)**:  
   Unlike traditional methods, the patent emphasizes edge-conditioned node transformations and scalar projections, providing a more expressive graph representation.  

2. **Hierarchical Pooling with Edge Awareness**:  
   The pooling mechanism retains critical nodes while accounting for edge features, resulting in a graph-level representation that preserves structural and relational information.  

3. **Improved Scalability and Efficiency**:  
   The hierarchical nature of graph coarsening reduces computational overhead while maintaining high accuracy, enabling real-time molecular property predictions.

---

### **Technical Effect**  

1. **Enhanced Prediction Accuracy**:  
   By leveraging both node and edge features, the system achieves superior performance in molecular property prediction tasks compared to baseline models.  

2. **Reduced Computational Cost**:  
   The hierarchical pooling and efficient edge-conditioned transformations lower computational complexity, making the system suitable for large-scale applications.  

3. **Robust Experimental Validation**:  
   Experimental results on the **QM-9 dataset** demonstrate significant performance improvements, with Mean Absolute Error (MAE) reduced for key properties like **HOMO**, **LUMO**, and **dipole moment**.  

---

### **Conclusion**  

This patent introduces a novel and technically advanced framework for molecular property prediction. By incorporating edge features \( e_{ij} \) in hierarchical graph pooling and feature transformation, the system overcomes limitations of conventional methods, achieving high accuracy and computational efficiency. These advancements hold significant potential for applications in drug discovery, material science, and beyond.
