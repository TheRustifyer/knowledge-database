# Ángulos coterminales

>[!summary] Definición
> Dos ángulos en posición estándar son coterminales si sus lados terminales coinciden; es decir, terminan apuntando en la misma dirección tras posiblemente haber dado distintas vueltas completas

## Obtención de ángulos coterminales

Para obtener un ángulo coterminal a otro ángulo cualquiera dado:
### En grados:

>[!tip] Ángulos n-coterminales en grados
>$$
\theta \pm 360^\circ \cdot n, \ n \in\mathbb{Z}
$$

>[!example] Ejemplo: Encuentra los tres menores ángulos positivos coterminales con 67º
>$$
>67^\circ + 360^\circ \cdot 1 = 67^\circ + 360^\circ = 427^\circ
>$$
>$$
>67^\circ + 360^\circ \cdot 2 = 67^\circ + 720^\circ = 787^\circ
>$$
>$$
>67^\circ + 360^\circ \cdot 3 = 67^\circ + 1080^\circ = 1147^\circ
>$$
### En radianes:

>[!tip] Ángulos n-coterminales en radianes
>$$
>\theta \pm n \cdot 2\pi, \ n \in \mathbb{Z}
>$$

>[!example] Ejemplo: Encuentra los dos menores ángulos positivos coterminales con $-3\pi/2$
>$$
>\frac{-3\pi}{2} + 1\cdot2\pi = \frac{-3\pi}{2} + 2\pi
>$$
>$$
>\frac{-3\pi}{2} + 1\cdot2\pi = \frac{-3\pi}{2} + 2\pi
>$$
>$$
>\frac{-3\pi}{2} + 2\pi \cdot \left(\frac{2}{2}\right)
>$$
>$$
>\frac{-3\pi}{2} + \frac{4\pi}{2}
>$$
>$$
>\frac{-\pi}{2}
>$$


>[!Recuerda]
>La obtención de cualquier ángulo coterminal negativo sigue el mismo proceso pero multiplicando el factor de coterminalidad n por -1

>[!warning] Factor de coterminalidad
>No sé si esa definición existe, pero la he designado yo para hablar sobre el factor $\ n \in \mathbb{Z}$ el cual indica las n-vuelta(s) completa(s) de 360º (o $2\pi$) desde un ángulo original a cualquiera de sus coterminales

### En DMS:

>[!tip] Ángulos n-coterminales en DMS
>Para encontrar ángulos coterminales cuando el ángulo está en DMS, se procede igual que con grados decimales, y simplemente se arrastran los minutos y segundos en cada suma o resta de vueltas completas.

> [!example] Ángulos coterminales en DMS — Rotaciones positivas
> Objetivo: hallar $\alpha$ coterminal con $150^\circ 17' 49''$ tras dos vueltas completas positivas.
>
> $$\alpha \;=\; 150^\circ 17' 49'' \;+\; 2\,(360^\circ)$$
> $$\alpha \;=\; 150^\circ 17' 49'' \;+\; 720^\circ$$
> $$\alpha \;=\; (150+720)^\circ\,17'\,49''$$
> $$\alpha \;=\; 870^\circ 17' 49''$$
>
> Comentario: signo del ángulo y de la rotación coinciden (positivos), por lo que se suman los grados y se “arrastran” minutos y segundos sin cambios.

---

> [!example] Ángulos coterminales en DMS — Rotaciones negativas y ajuste de signos
> Objetivo: hallar $\alpha$ coterminal con $16^\circ 20' 42''$ tras tres vueltas completas negativas.
>
> Partimos de:
> $$\alpha \;=\; (16^\circ + 20' + 42'') \;-\; 3\,(360^\circ)$$
> $$\alpha \;=\; 16^\circ + 20' + 42'' \;-\; 1080^\circ$$
> Reagrupando grados por un lado y minutos/segundos por otro:
> $$\alpha \;=\; (16^\circ - 1080^\circ) \;+\; 20' \;+\; 42''$$
> $$\alpha \;=\; -1064^\circ \;+\; 20' \;+\; 42''$$
>
> En DMS, los tres componentes deben compartir el mismo signo. Convertimos $20'$ y $42''$ a negativos usando “préstamos”.
>
> 1) Hacer los minutos negativos: tomar $-1^\circ$ de los grados y pasarlo a minutos ($1^\circ = 60'$).
> $$-1064^\circ \;=\; -1063^\circ \;+\; (-1^\circ)$$
> $$(-1^\circ) \;=\; -60'$$
> Sustituyendo:
> $$\alpha \;=\; -1063^\circ \;+\; (-60') \;+\; 20' \;+\; 42''$$
> $$\alpha \;=\; -1063^\circ \;+\; (-40') \;+\; 42''$$
>
> 2) Hacer los segundos negativos: tomar $-1'$ de los minutos y pasarlo a segundos ($1' = 60''$).
> $$-40' \;=\; -39' \;+\; (-1')$$
> $$(-1') \;=\; -60''$$
> Sustituyendo:
> $$\alpha \;=\; -1063^\circ \;+\; (-39') \;+\; (-60'') \;+\; 42''$$
> $$\alpha \;=\; -1063^\circ \;-\; 39' \;-\; 18''$$
>
> Resultado final coherente en DMS (todo negativo):
> $$\boxed{\;\alpha \;=\; -1063^\circ\ 39'\ 18''\;}$$
>
> Nota: El signo negativo delante implica que grados, minutos y segundos pertenecen a una rotación global negativa; los tres componentes comparten el signo tras los “préstamos”.
