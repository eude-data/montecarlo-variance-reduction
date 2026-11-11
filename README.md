# Monte Carlo et réduction de variance

Projet réalisé en M2 ISIFAR (Université Paris Cité). L'objectif est de comparer l'estimateur Monte Carlo classique à deux techniques de réduction de variance, les variables antithétiques et l'échantillonnage préférentiel (importance sampling), puis de pricer une option barrière par simulation de trajectoires.

Tout le code est dans `montecarlo.ipynb`. Pour chaque méthode, l'estimation est répétée plusieurs centaines de fois et on compare les histogrammes obtenus.

## 1. Calcul d'une intégrale

On estime

$$I = \int_0^{10} x^2 e^x\, dx = 82\, e^{10} - 2 \approx 1\,806\,168$$

En posant x = 10u, on a I = 10 E[f(10U)] avec U uniforme sur [0, 1], d'où l'estimateur simple calculé sur 10 000 tirages.

La version antithétique utilise la même série de tirages U et leur symétrique 1 − U :

$$\hat{I}_{\text{anti}} = \frac{10}{2n} \sum_{i=1}^{n} \left[f(10U_i) + f(10(1 - U_i))\right]$$

En relançant le code sur 100 répétitions, l'écart-type passe d'environ 40 000 à environ 22 000.

## 2. Variables antithétiques sur un call européen

Call de Black-Scholes avec S₀ = 10, r = 0, σ = 0.2, T = 1. On remplace le brownien W(T) par −W(T) pour obtenir une seconde trajectoire, puis on fait la moyenne des deux payoffs.

- Call à la monnaie (K = 10) : l'histogramme de l'estimateur antithétique est nettement plus resserré, avec un écart-type environ divisé par 2.
- Call très en dehors de la monnaie (K = 25) : la méthode ne sert plus à rien. Le strike n'est presque jamais atteint et 96.2 % des estimations sont exactement nulles.

## 3. Importance sampling

Ce dernier cas motive le changement de mesure. On simule le sous-jacent avec un drift α plus grand que r, ce qui envoie beaucoup plus de trajectoires au-dessus du strike, puis on corrige chaque payoff par la densité de Radon-Nikodym. Avec θ = (α − r)/σ :

$$\hat{C}_{\text{IS}} = e^{-rT}\, \frac{1}{n} \sum_{i=1}^{n} \left(S_T^{(i)} - K\right)^+ \exp\left(-\theta W_T^{(i)} - \frac{\theta^2 T}{2}\right)$$

Test sur un call avec K = 20 (S₀ = 10, σ = 0.2, T = 1), dont le prix exact vaut 0.000189. Avec α = 0.5, l'importance sampling sur 10 000 tirages donne une dispersion comparable à celle de l'estimateur simple sur 10 millions de tirages, soit environ 1 000 fois moins de simulations.

## 4. Option barrière : put down-and-in

Le put n'est activé que si le sous-jacent passe sous la barrière B avant l'échéance :

$$\text{payoff} = (K - S_T)^+\, \mathbf{1}_{\left\lbrace \min_{t \le T} S_t \le B \right\rbrace}$$

Les trajectoires sont simulées sur 1 000 pas de temps. Avec S₀ = K = 10, B = 6, r = 0, σ = 0.2 et T = 1, on obtient un prix d'environ 0.055. Le minimum n'est observé qu'aux dates de la grille, donc l'estimateur sous-estime légèrement la probabilité de toucher la barrière.

## Récapitulatif

| Cas | Méthode | Tirages | Résultat |
|---|---|---|---|
| Intégrale | simple, puis antithétique | 10 000 | écart-type divisé par 2 environ |
| Call K = 10 | simple, puis antithétique | 10 000 | écart-type divisé par 2 environ |
| Call K = 25 | antithétique | 10 000 | 96.2 % d'estimations nulles |
| Call K = 20 | simple | 10 000 000 | dispersion de référence |
| Call K = 20 | importance sampling | 10 000 | dispersion comparable |
| Put down-and-in | trajectoires, 1 000 pas | 10 000 | environ 0.055 |

Bibliothèques : numpy, scipy, matplotlib.
