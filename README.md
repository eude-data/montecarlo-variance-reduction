# Monte Carlo — Réduction de Variance 🎲

Implémentation et comparaison de techniques de réduction de variance pour l'estimation Monte Carlo, appliquées au pricing d'options sous Black-Scholes.

---

## Contenu

### `montecarlo.ipynb`

#### 1. Monte Carlo naïf

Estimation de $\int_0^{10} x^2 e^x\, dx$ par simulation uniforme. Mise en évidence de la variance de l'estimateur de base.

#### 2. Variables antithétiques

Principe : remplacer $U$ par $(U + (1-U))/2$ pour exploiter la corrélation négative.

```
Estimateur antithétique : ½ [f(U) + f(1-U)]
```

Comparaison sur call vanille ATM — réduction significative de la variance par rapport à l'estimateur simple.

#### 3. Importance Sampling (IS)

Changement de mesure pour les options très hors de la monnaie (deep OTM), où $\mathbb{P}(S_T > K) \ll 1$.

On simule sous une mesure $\tilde{\mathbb{P}}$ avec drift $\alpha > r$, puis on corrige par le rapport de Radon-Nikodym :

$$\hat{C}_{IS} = e^{-rT} \cdot \frac{1}{N} \sum_{i=1}^N (S_T^i - K)^+ \cdot \exp\!\left(-\frac{\alpha - r}{\sigma} W_T^i - \frac{1}{2}\left(\frac{\alpha-r}{\sigma}\right)^2 T\right)$$

Comparaison IS vs estimateur naïf sur un call deep OTM ($K = 20$, $S_0 = 10$) — gain en variance spectaculaire avec $10^4$ simulations contre $10^7$ pour l'estimateur simple.

#### 4. Option barrière — Put Down-and-In (PDI)

Pricing d'un put avec barrière activante inférieure par simulation de trajectoires complètes :

$$\text{Payoff} = (K - S_T)^+ \cdot \mathbf{1}_{\min_{t \leq T} S_t \leq B}$$

Discrétisation de $[0, T]$ en 1000 pas, avec visualisation des trajectoires.

---

## Résultats illustratifs

| Méthode | Option | Nb simulations | Variance relative |
|---|---|---|---|
| Monte Carlo naïf | Call ATM | 10 000 | référence |
| Variables antithétiques | Call ATM | 10 000 | ↓ significative |
| MC naïf | Call OTM ($K=20$) | 10 000 000 | élevée |
| Importance Sampling | Call OTM ($K=20$) | 10 000 | ↓↓ drastique |

---

## Concepts clés

- **Estimateur Monte Carlo** et loi des grands nombres
- **Variables antithétiques** : exploitation de la corrélation négative
- **Importance Sampling** : changement de mesure, théorème de Girsanov
- **Options barrières** : simulation de trajectoires, temps de premier passage

---

## Stack

```python
langages   = ["Python"]
librairies = ["numpy", "scipy", "matplotlib"]
```
