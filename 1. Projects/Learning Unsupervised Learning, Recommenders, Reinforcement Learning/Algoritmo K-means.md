Es un algoritmo de clasificación no supervisada ([[Clustering]]), que agrupa objetos en k grupos basándose en sus características. El agrupamiento se realiza minimizando la suma de distancias entre cada objeto y el centroide de su cluster. Se suele usar la **distancia cuadrática**.

El algoritmo consta de tres pasos:
1. **Inicialización**: una vez escogido el número de grupos, k, se establecen k centroides en el espacio de los datos, por ejemplo, escogiéndolos aleatoriamente.
2. **Asignación objetos a los centroides**: cada objeto de los datos es asignado a su centroide más cercano.
3. **Actualización centroides**: se actualiza la posición del centroide de cada grupo tomando como nuevo centroide la posición del promedio de los objetos pertenecientes a dicho grupo.

Se repiten los pasos 2 y 3 hasta que los centroides no se mueven, o se mueven por debajo de una distancia umbral.

Este algoritmo resuelve un problema de optimización, siendo la función a optimizar la suma de las distancias cuadráticas de cada objeto al centroide de su cluster.

El algoritmo sería de la siguiente manera en pseudocódigo:

```LUA
1. Initialize centroids
   - Randomly select k data points from the dataset as initial centroids.

2. Repeat until convergence:
   a. Assignment step:
      - For each data point in the dataset:
        i.  Calculate the distance between the data point and each centroid.
        ii. Assign the data point to the nearest centroid.
   
   b. Update step:
      - For each centroid:
        i.  Calculate the new centroid by taking the mean of all data points assigned to it.

3. Convergence criteria:
   - Check if the centroids have stopped moving (i.e., the changes in centroid positions are below a certain threshold).
   - If centroids have converged, terminate the algorithm.
   - If not, repeat steps 2a and 2b.

End Algorithm
}
```

a$^{1}$$_{more text}$

En Python, para hacer el segundo paso tendríamos:

```Python
import numpy as np
import matplotlib.pyplot as plt
from utils import *

def find_closest_centroids(X, centroids):
    """
    Computes the centroid memberships for every example
    
    Args:
        X (ndarray): (m, n) Input values      
        centroids (ndarray): (K, n) centroids
    
    Returns:
        idx (array_like): (m,) closest centroids
    
    """

    # Set K
    K = centroids.shape[0]

    # You need to return the following variables correctly
    idx = np.zeros(X.shape[0], dtype=int)

    for i in range(X.shape[0]):
        # squared Euclidean distance
        distances = np.sum((X[i] - centroids) ** 2, axis=1)
        idx[i] = np.argmin(distances)

    return idx
```
En este algoritmo, aplicamos la función $c^{(i)}$ := j que minimiza $|| c^{(i)} - \mu_j ||$, donde $c^{(i)}$ es el índice del centroide que es más cercano a $x^{(i)}$, $\mu_j$ es la posición del j'th centroide.

Ahora, veamos el algoritmo para calcular los nuevos centroides:
```Python
def compute_centroids(X, idx, K):
    """
    Returns the new centroids by computing the means of the 
    data points assigned to each centroid.
    
    Args:
        X (ndarray):   (m, n) Data points
        idx (ndarray): (m,) Array containing index of closest centroid for each 
                       example in X. Concretely, idx[i] contains the index of 
                       the centroid closest to example i
        K (int):       number of centroids
    
    Returns:
        centroids (ndarray): (K, n) New centroids computed
    """
    
    # Useful variables
    m, n = X.shape
    
    # You need to return the following variables correctly
    centroids = np.zeros((K, n))
    
    for k in range(K):
        points = X[idx == k]
        if len(points) > 0:
            centroids[k] = np.mean(points, axis=0) 
    
    return centroids
```
Esto representa la siguiente ecuación: $\mu_k = \frac{1}{|C_k|} \sum_{i \in C_k} x^{(i)}$
Donde $C_k$ es el set de datos que está asignado a un centroide $k$, y $|C_k|$ es el número de datos en el set $C_k$.

Todo esto está sacado de un cuaderno de Jupyter del curso "Unsupervised Learning, Recommenders, Reinforcement Learning" de DeepLearning.AI.

Aquí está el cuaderno de Jupyter: ![[C3_W1_KMeans_Assignment.ipynb]]

