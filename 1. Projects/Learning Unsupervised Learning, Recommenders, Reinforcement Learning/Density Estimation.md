 # ¿Qué es?
La estimación de densidad es un algoritmo de[[Detección de anomalías | detección de anomalías]].
Pongamos que tenemos un dataset: $\{\bar{\mathbf{x}}^{(1)}, \bar{\mathbf{x}}^{(2)}, ..., \bar{\mathbf{x}}^{(m)}\}$
Cada ejemplo $\bar{\mathbf{x}}^{(i)}$ tiene $n$ features. 

Tendremos que la probabilidad de x será:

$p(\bar{\mathbf{x}}) = p(x_1; \mu_1, \sigma_1²) * p(x_2; \mu_2, \sigma_2²    )  *  ...  *  p(x_n; \mu_n, \sigma_n²) = \prod_{j=1}^n p(x_j; \mu_j, \sigma_j^2)$

1. Choose $n$ features $x_i$ that you think might be indicative of anomolous examples.
2. Fit parameters $\mu_1, ... \mu_n, \sigma_1², ... \sigma_n²$

$\mu_j = \frac{1}{m} \sum_{i=1}^m x_j^{(i)}$       $\sigma_j^2 = \frac{1}{m} \sum_{i=1}^m (x_j^{(i)} - \mu_j)^2$

$\bar{\mu} = \frac{1}{m} \sum_{i=1}^m \bar{x}^{(i)}$          $\bar{\mu} = \begin{bmatrix} \mu_1 \\ \mu_2 \\ \vdots \\ \mu_n \end{bmatrix}$
3. Given new example $x$, compute $p(x)$:

$p(\mathbf{x}) = \prod_{j=1}^n p(x_j; \mu_j, \sigma_j^2) = \prod_{j=1}^n \frac{1}{\sqrt{2\pi\sigma_j^2}} \exp(-\frac{(x_j - \mu_j)^2}{2\sigma_j^2})$

Anomaly if $p(x) < \epsilon$ ![[Pasted image 20241001185611.png]]

Lo que hace este algoritmo es calcular la campana de Gauss o[[Distribución normal (Gaussian) | distribución normal]] del dataset.

## Implementación
Veamos como implementar este algoritmo en la realidad.
Asumamos que tenemos algunos datos etiquetados, de anomalías ($y=1$) y de no anomalías ($y=0$).
Nuestro training set va a tener $y=0$ para todos los ejemplos del dataset.

Un ejemplp de una implementación de este algoritmo en: ![[C3_W1_Anomaly_Detection.ipynb]]