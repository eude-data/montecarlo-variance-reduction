# Monte Carlo - Réduction de Variance 🎲

Implémentation et comparaison de techniques de réduction de variance pour l'estimation Monte Carlo, appliquées au pricing d'options sous Black-Scholes.

---

## Contenu

### `montecarlo.ipynb`

#### 1. Monte Carlo naïf

Estimation d'une intégrale par simulation uniforme. Mise en évidence de la variance de l'estimateur de base.

#### 2. Variables antithétiques

Pour chaque simulation $U$, on calcule également $f(1-U)$ et on moyenne les deux — la corrélation négative entre les deux tirages réduit significativement la variance de l'estimateur.

Comparaison sur call vanille ATM : réduction nette de la dispersion par rapport au Monte Carlo simple.

#### 3. Importance Sampling

Changement de mesure pour les options deep OTM, où la probabilité d'exercice est très faible et l'estimateur naïf est très inefficace.

On simule sous une mesure auxiliaire avec drift plus élevé, puis on corrige par le rapport de Radon-Nikodym (théorème de Girsanov). Résultat : 10 000 simulations suffisent là où l'estimateur naïf en nécessite 10 000 000 pour la même précision.

#### 4. Option barrière — Put Down-and-In (PDI)

Pricing d'un put activé lorsque le sous-jacent franchit une barrière basse. On simule des trajectoires complètes discrétisées en 1000 pas sur $[0, T]$, et on vérifie si le minimum de la trajectoire atteint la barrière $B$.

---

## Résultats illustratifs

| Méthode | Option | Simulations | Variance |
|---|---|---|---|
| Monte Carlo naïf | Call ATM | 10 000 | référence |
| Variables antithétiques | Call ATM | 10 000 | ↓ significative |
| MC naïf | Call OTM (K=20) | 10 000 000 | élevée |
| Importance Sampling | Call OTM (K=20) | 10 000 | ↓↓ drastique |

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
