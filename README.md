# Assignment-2: Learning Probability Density Function using GAN

## 1. Objective
The goal of this assignment was to learn an unknown probability density function (PDF) of a transformed random variable using only data.  
Since no analytical PDF was provided, a Generative Adversarial Network (GAN) was used to learn the distribution of the transformed values.

---

## 2. Dataset
I used the NO₂ concentration values from the India Air Quality dataset.  
The `no2` column was cleaned, converted to numeric, and missing values were removed.

---

## 3. Transformation of Data
Each value of `x` was transformed using the formula:

\[
z = x + a_r \cdot \sin(b_r \cdot x)
\]

where:

- \( a_r = 0.5 \times (r \mod 7) \)
- \( b_r = 0.3 \times (r \mod 5 + 1) \)
- `r` is my university roll number  

This transformation introduces non-linearity, making the true PDF unknown.

---

## 4. GAN Architecture

### **Generator**
- Input: random noise sampled from \( N(0,1) \)  
- Dense layers + LeakyReLU activations  
- Output: a single generated value approximating the transformed variable

### **Discriminator**
- Input: real `z` samples and fake generated samples  
- Dense layers + LeakyReLU  
- Output: probability of the sample being real  

### **Training Details**
- Loss: Binary Cross Entropy  
- Optimizer: Adam (lr = 0.0002)  
- Trained for 5000 epochs  
- Batch size: 64  

The discriminator learns to separate real vs fake samples, while the generator learns to fool the discriminator.

---

## 5. Generated Samples
After training, I sampled thousands of points from the generator:

\[
z_f = G(\epsilon), \quad \epsilon \sim N(0,1)
\]

These generated points were then used for PDF estimation.

---

## 6. PDF Estimation
Since the true PDF is unknown, I estimated it using:

- Histogram density estimation  
- Kernel Density Estimation (KDE) with Gaussian kernel  

The KDE plot produced a continuous and smooth estimate of the learned PDF.

---

## 7. Observations

### **Mode Coverage**
The generator captured the overall shape of the real transformed distribution.  
The generated samples covered the major density regions well.

### **Training Stability**
- During initial epochs, losses fluctuated  
- After enough iterations, training became stable  
- No severe mode collapse occurred  

### **Quality of PDF Estimation**
- The final KDE curve resembled the empirical distribution of the transformed data  
- Peaks and spread were learned reasonably well by the GAN

---

## 8. Final Output Included
- Transformation parameters \( a_r \) and \( b_r \)
  a_r = 2.5
  b_r = 1.5
- GAN architecture description  
- Training loss values  
- First few generated sample values  
- Final PDF plot (Histogram/KDE) obtained from GAN samples
  <img width="576" height="455" alt="image" src="https://github.com/user-attachments/assets/25c882bf-743f-42e0-9702-bcd9fc4cee1c" />
  <img width="576" height="455" alt="image" src="https://github.com/user-attachments/assets/92214bfd-dc50-40cc-a95c-4369126c0ff2" />

---

## 9. Libraries Used
- Python  
- NumPy, Pandas  
- Matplotlib  
- Scikit-learn (KDE)  
- TensorFlow / Keras  

---

## 10. Conclusion
A GAN was successfully used to learn and approximate the unknown probability density function of the transformed variable.  
The generated PDF matches the general behaviour of the transformed data, showing that the GAN could effectively model the underlying distribution.

