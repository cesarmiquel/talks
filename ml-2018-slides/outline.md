# Machine Learning

## Abstract
Los últimos 6 años hemos visto un gran progreso en inteligencia artificial y aprendizaje automático. En
esta charla vamos a revisar la historia reciente, ver varios de estos avances e intentar entender a qué
se deben. Haremos un repaso de los distintos tipos de redes neuronales más comunes y sus aplicaciones en
problemas actuales. Finalmente voy a compartir algunas herramientas disponibles hoy en día para
incursionar en éste tema.


## Slide 1

What is Machine Learning? Let's see two definitions:

- Arthur Samuel (1959). Machine Learning: Field of study that gives computers the ability to learn
  without being explicitly programmed.

- Tom Mitchell (1998) Well-posed Learning Problem: A computer program is said to learn from experience
  E with respect to some task T and some performance measure P, if its performance on T, as measured
  by P, improves with experience E.


*Notes:* Samuel desarrolló un programa para jugar a las damas. Instead of searching each path until it
came to the game’s conclusion, Samuel developed a scoring function based on the position of the board
at any given time. This function tried to measure the chance of winning for each side at the given position.
It took into account such things as the number of pieces on each side, the number of kings, and the proximity
of pieces to being “kinged”. The program chose its move based on a minimax strategy, meaning it made the move
that optimized the value of this function, assuming that the opponent was trying to optimize the value of the
same function from its point of view.

## Slide 2

Algunos ejemplos:

1. Estimar el precio de una casa en función de los metros cuadrados,


## Slide n

Explicación NN. Auto-encoder:

[cs294a Sparse Autoencoder Lecture Part 1 - Andrew Ng](https://www.youtube.com/watch?v=vfnxKO2rMq4)
[Variational Autoencoders](https://www.youtube.com/watch?v=9zKuYvjFFS8)
