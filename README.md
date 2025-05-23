## Trabajo Practico 2:

**Conclusiones sobre técnicas de Data Augmentation en CIFAR-10**

Se realizaron múltiples experimentos evaluando distintas combinaciones de técnicas de data augmentation sobre un modelo Vision Transformer (ViT), entrenado con el dataset CIFAR-10. A continuación, se detallan las principales observaciones:

**Modelo reducido por limitaciones prácticas:** Se utilizó una arquitectura ViT pequeña debido a restricciones de cómputo. El enfoque estuvo centrado en evaluar el comportamiento del código y el impacto de las distintas técnicas de data augmentation, más que en alcanzar el mayor rendimiento posible.

**Impacto inicial positivo:** La aplicación de augmentations simples como RandomHorizontalFlip y RandomResizedCrop (v1 y v2) mejoró ligeramente las métricas de desempeño en comparación con el modelo base sin augmentations (v0). Esto sugiere una mejor generalización al incorporar pequeñas variaciones espaciales.

**Saturación con aumentos excesivos:** Al incrementar la complejidad de los augmentations (v3 y v4), incluyendo ColorJitter y RandomRotation, se observó un aumento considerable del tiempo de entrenamiento por época, pero sin mejoras significativas en las métricas. En algunos casos, incluso se degradó el desempeño.

**AutoAugment no funcionó como se esperaba:** La política AutoAugmentPolicy.CIFAR10 (v5), si bien genera transformaciones más sofisticadas, resultó en una pérdida más alta y menor precisión. Esto podría deberse a la sensibilidad del ViT frente a perturbaciones intensas, especialmente en etapas iniciales del entrenamiento.

**Trade-off entre desempeño y tiempo:** Las técnicas más simples ofrecen un buen equilibrio entre mejora de desempeño y costo computacional. En cambio, las más agresivas incrementan el tiempo sin garantía de beneficios claros.

En la tabla debajo se observan los resultados de cada una de las pruebas.

<span style="font-size:16px">

| Experimento | Augmentations aplicadas            | Tiempo/época (s) | Loss final | Accuracy | Precision | Recall | F1   |
| ----------- | ---------------------------------- | ---------------- | ---------- | -------- | --------- | ------ | ---- |
|v0           | Ninguna                            | 20.0              | 0.5577       | 0.63     | 0.62      | 0.63   | 0.62 |
|v1           | Flip horizontal                    | 22.0              | 0.7678       | 0.65     | 0.65      | 0.65   | 0.65 |
|v2           | Flip + Crop                        | 36.0              | 0.8655       | 0.66     | 0.67      | 0.66   | 0.66 |
|v3           | Flip + Crop + ColorJitter          | 59.0              | 0.8542       | 0.65     | 0.65      | 0.65   | 0.65 |
|v4           | Flip + Crop + ColorJitter + RandomRotation | 68.0             | 1.0886      | 0.62     | 0.63      | 0.62   | 0.62 |
|v5           | AutoAugmentPolicy.CIFAR10 | 41.0             | 1.0734      | 0.56     | 0.56      | 0.56   | 0.56 |

</span>