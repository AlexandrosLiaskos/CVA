# Coastal Vulnerability Formalization

## Core Definition
"Coastal vulnerability refers to the expected amount of damage that a coastal ecosystem will face as the outcome from its exposure and lack of resilience to coastal hazards"

### Formal Mathematical Structure
$$CV(ce) = \mathbb{E}_{h \sim P(h|e_{ch})}[D(ce, h, lor_{ch}(ce))]$$
Where:

- CV(ce) = Coastal vulnerability of coastal ecosystem ce
- $\mathbb{E}_{h \sim P(h|e_{ch})}[·]$ = Expected value over hazard scenarios h distributed according to exposure characteristics
- D(·) = Damage function mapping (ecosystem, hazard realization, resilience) → damage
- ce = Coastal ecosystem state vector
- h = Specific hazard realization (random variable)
- $P(h|e_{ch})$ = Probability distribution of hazard scenarios conditional on exposure characteristics
- $lor_{ch}(ce)$ = Lack of resilience function dependent on ecosystem state

## Term-by-Term Formalization

#### 0. **Coastal Ecosystem (ce)**
- **Definition**: The specific coastal ecosystem under assessment, characterized by its geographical, biological, physical, and functional attributes.
- **Mathematical Representation**: A tuple or set of features.
  $$ ce = (G, B, P, F) $$
  Where:
  - $G$: Geographic attributes (e.g., location coordinates, spatial extent/area $A_{ce}$, boundaries, connectivity to other ecosystems).
  - $B$: Biotic attributes (e.g., species composition, biodiversity metrics, population densities, habitat types like mangrove, coral reef, saltmarsh, seagrass bed).
  - $P$: Physical/Abiotic attributes (e.g., geomorphology, sediment type, bathymetry, water quality parameters, structural complexity).
  - $F$: Functional attributes (e.g., ecosystem services provided like coastal protection, carbon sequestration rate, nursery function, recreational value).
- **Domain**:
  - $G \in \text{SpatialObjects}$
  - $B \in \text{BioticFeatureSpace}$
  - $P \in \text{PhysicalFeatureSpace}$
  - $F \in \text{FunctionalFeatureSpace}$
- **Notes**:
  - The specific attributes chosen for $B, P, F$ will depend on the assessment's scope and the type of damage being quantified.
  - `ce` properties directly inform `lor_ch` (e.g., $A(ce), R(ce), S(ce)$) and `Ψ(ce)` in the damage function.

#### 1. **Coastal Vulnerability**
- **Definition**: The primary metric representing the anticipated impact on a coastal ecosystem
- **Mathematical Representation**:
  $$ \text{CV}(ce) = \mathbb{E}_{h \sim P(h|e_{ch})}[D(ce, h, \text{lor}_{ch}(ce))] $$
- **Domain**: $\mathbb{R}_{\geq 0}$ (non-negative real numbers)
- **Constraints**:
  - $\text{CV}(ce) \geq 0$ (non-negativity)
  - $\text{CV}(ce) \leq \phi(ce)$ where $\phi(ce)$ is the maximum possible damage (e.g., total ecosystem area, biomass, or functional capacity)

#### 2. **Expected Amount of Damage**
- **Definition**: Statistical expectation of damage over all possible hazard scenarios
- **Mathematical Representation**:
  $$\mathbb{E}_{h \sim P(h|e_{ch})}[D] = \int_{\mathcal{H}} p(h|e_{ch}) \cdot D(ce, h, lor_{ch}(ce))  dh $$
  Where:
  - $\mathcal{H}$: Space of all hazard scenarios $h$
  - $p(h|e_{ch})$: Conditional probability density function of hazard $h$ given exposure characteristics
  - $D(ce, h, lor_{ch}(ce))$: Damage function for specific hazard realization $h$
- **Properties**:
  - Probabilistic integration over uncertainty in hazards
  - Future-oriented (models stochastic future events)
  - Dimensionally consistent (units depend on damage metric chosen)
- **Discrete Event Alternative**:
  $$CV(ce) = \sum_{i \in T} \lambda_i \cdot \mathbb{E}[D(ce, h_i, lor_{ch}(ce))]$$
  Where $\lambda_i$ is the annual frequency of hazard type $i$ and $T$ is the set of discrete hazard types.

#### 3. **Exposure to Coastal Hazards**
- **Definition**: Characterization of hazard probability distributions affecting the ecosystem
- **Mathematical Representation**:
  $$ e_{ch} = \left\{ (\lambda_i, F_i) \right\}_{i \in T} $$
  Where:
  - $\lambda_i$: Annual frequency of hazard type $i$ (events/year)
  - $F_i(h)$: Cumulative distribution function of intensity for hazard type $i$
  - $T$: Set of relevant hazard types (e.g., storms, sea-level rise)
- **Hazard Realization Sampling**:
  $$h \sim P(h|e_{ch}) = \sum_{i \in T} \frac{\lambda_i}{\sum_j \lambda_j} \cdot f_i(h)$$
  Where $f_i(h)$ is the probability density function derived from $F_i(h)$
- **Domain**:
  - $\lambda_i > 0$ (positive real), $F_i$ is a valid CDF
- **Constraints**:
  - $\sum_i \lambda_i < \infty$ (finite total hazard rate)
- **Ecosystem Feedback**: $e_{ch} = F_{EH}(ce)$ where ecosystem state modifies exposure through protective services

#### 4. **Lack of Resilience**
- **Definition**: Inability to absorb/recover from hazards, quantified via *resilience factors*
- **Mathematical Representation**:
  $$lor_{ch}(ce) = 1 - \left( w_1 \cdot A(ce) + w_2 \cdot R(ce) + w_3 \cdot S(ce) \right)$$
  Where:
  - $A(ce) \in [0,1]$: Adaptive capacity (e.g., species diversity)
  - $R(ce) \in [0,1]$: Recovery potential (e.g., sediment replenishment rate)
  - $S(ce) \in [0,1]$: Structural integrity (e.g., mangrove density)
  - $w_j$: Weights where $\sum w_j = 1$
- **Domain**: $\text{lor}_{ch} \in [0, 1]$
  (0 = fully resilient, 1 = no resilience)
- **Critical Constraint**:
  Damage $D$ must be a monotonically non-decreasing function of $lor_{ch}(ce)$ (higher lack of resilience → higher damage)
- **Weight Determination**: Use multi-criteria decision analysis (MCDA) or expert elicitation with consistency checks
- **Sensitivity Analysis**: Mandatory sensitivity analysis on all weight parameters ($w_j$) to assess robustness of $lor_{ch}$ calculations

#### 5. **Damage Function**
- **Definition**: Mechanism translating hazard impact into quantifiable ecosystem damage
- **Mathematical Representation** (One Possible Functional Form):
  $$ D(ce, h, lor_{ch}(ce)) = f(h) \cdot g(lor_{ch}(ce)) \cdot \Psi(ce) $$
  Where:
  - $f(h)$: Hazard intensity multiplier (e.g., $\text{storm surge height}^2$)
  - $g(lor_{ch}(ce))$: Resilience scaling term (e.g., $e^{k \cdot lor_{ch}(ce)}$ with $k > 0$)
  - $\Psi(ce)$: Ecosystem's maximum damage potential (e.g., total area, biomass, or service value)
- **Alternative Forms**: Additive models ($D = f(h) + g(lor_{ch}) + \Psi(ce)$) or more complex interaction terms may be appropriate depending on the specific ecosystem-hazard system
- **Domain**: $\mathbb{R}_{\geq 0}$ (non-negative)
- **Enforced Constraints**:
  - $\frac{\partial D}{\partial h} > 0$, $\frac{\partial D}{\partial lor_{ch}} > 0$ (damage monotonicity)
  - $D(ce, h, lor_{ch}(ce)) \leq \phi(ce)$ ∀ h (upper physical bound enforced via saturation)
  - $\lim_{h \to \infty} D(ce, h, lor_{ch}(ce)) = \phi(ce)$ (damage approaches but never exceeds maximum)

#### 6. **Spatio-Temporal Dynamics**
- **Definition**: Accounting for changes in coastal vulnerability over a defined time horizon and across spatial extents.
- **Mathematical Representation**:
    - **Time Dependence**: Key variables become functions of time $t \in [0, T_H]$, where $T_H$ is the time horizon.
      - Ecosystem: $ce(t) = (G(t), B(t), P(t), F(t))$
      - Exposure: $e_{ch}(t) = \left\{ \lambda_i(t), I_i(t) \right\}_{i \in T}$
      - Lack of Resilience: $\text{lor}_{ch}(t) = 1 - \left( w_1 \cdot A(ce(t)) + w_2 \cdot R(ce(t)) + w_3 \cdot S(ce(t)) \right)$
      - **Per-Event Expected Damage** at time $t$: $D_{event}(t) = \mathbb{E}_{h \sim P(h|e_{ch}(t))}[D(ce(t), h, lor_{ch}(ce(t)))]$
      - **Annual Expected Damage** at time $t$: $D_{annual}(t) = \sum_{i \in T} \lambda_i(t) \cdot \mathbb{E}_{h \sim F_i(t)}[D(ce(t), h, lor_{ch}(ce(t)))]$
        Where $\lambda_i(t)$ is the annual frequency of hazard type $i$ at time $t$
    - **Vulnerability over Time Horizon** (Discrete Time Framework):
      - **Average Annual Vulnerability**:
        $$ CV_{T_H, avg}(ce) = \frac{1}{T_H} \sum_{t=1}^{T_H} D_{annual}(t) $$
      - **Discounted Total Expected Damage**:
        $$ CV_{T_H, total}(ce) = \sum_{t=1}^{T_H} \frac{1}{(1+r)^t} D_{annual}(t) $$
        Where $r$ is the discount rate for temporal preferences
    - **Spatial Explicitness**: Variables become functions of spatial coordinates $\mathbf{s} = (x,y)$ (or a more general spatial vector) within the ecosystem's spatial domain $\mathcal{S}_{ce}$.
      - $ce(\mathbf{s})$, $e_{ch}(\mathbf{s})$, $\text{lor}_{ch}(\mathbf{s})$
      - Spatially Explicit Vulnerability:
        $$ CV(ce, \mathbf{s}) = \mathbb{E}[D(ce(\mathbf{s}), e_{ch}(\mathbf{s}), \text{lor}_{ch}(\mathbf{s}))] $$
        This results in a vulnerability map.
    - **Combined Spatio-Temporal Vulnerability**:
      - $ce(\mathbf{s}, t)$, $e_{ch}(\mathbf{s}, t)$, $\text{lor}_{ch}(\mathbf{s}, t)$
      - Instantaneous Spatially Explicit Vulnerability:
        $$ CV(ce, \mathbf{s}, t) = \mathbb{E}[D(ce(\mathbf{s}, t), e_{ch}(\mathbf{s}, t), \text{lor}_{ch}(\mathbf{s}, t))] $$
- **Domain**:
  - $t \in [0, T_H]$
  - $\mathbf{s} \in \mathcal{S}_{ce} \subset \mathbb{R}^2$ (or $\mathbb{R}^3$)
- **Stability Analysis for Feedback Systems**:
  - System converges if $\|F_{EH}(ce_{t+1}) - F_{EH}(ce_t)\| < \epsilon$ for ecosystem feedback
  - Lyapunov stability condition: $\frac{d}{dt}V(ce(t)) < 0$ where $V$ is a suitable energy function
- **Notes**:
  - Discrete time framework better represents annual assessment cycles
  - Discounting prevents unbounded damage accumulation over infinite horizons
  - Spatially explicit models require gridded or vector data for inputs

#### 7. **Interdependencies and Feedbacks**
- **Definition**: Interactions where ecosystem state influences hazard exposure, or where damage/recovery dynamics are complex, or where ecosystems influence each other's vulnerability.
- **Mathematical Representation**:
    - **Ecosystem-Hazard Feedback**: Exposure $e_{ch}$ becomes a function of the ecosystem state $ce$.
      $$ e_{ch} = \mathcal{F}_{EH}(ce) $$
      Where $\mathcal{F}_{EH}$ is a function or model translating ecosystem attributes (e.g., reef height, mangrove density) into modified hazard characteristics (e.g., reduced wave energy).
      The core CV equation becomes: $CV(ce) = \mathbb{E}_{h \sim P(h|\mathcal{F}_{EH}(ce))}[D(ce, h, lor_{ch}(ce))]$.
    - **Damage Accumulation & Recovery Dynamics**: The state of damage $D_{state}$ evolves over time with explicit feedback to ecosystem state.
      - Let $D_{state}(t)$ be the accumulated damage at time $t$.
      - For a hazard event $h_k$ at time $t_k$, instantaneous damage $D_{event,k} = D(ce(t_k), h_k, \text{lor}_{ch}(t_k))$.
      - Recovery function $Rec(ce, D_{state}, \Delta t)$ describes damage reduction over $\Delta t$.
      - Evolution of damage (discrete time):
        $$ D_{state}(t + \Delta t) = D_{state}(t) + \sum_{k \text{ s.t. } t_k \in [t, t+\Delta t)} D_{event,k} - Rec(ce(t), D_{state}(t), \Delta t) $$
      - **Critical Feedback**: Ecosystem state and resilience depend on damage state:
        - $ce(t) = \mathcal{F}_{damage}(ce_0, D_{state}(t))$ where $ce_0$ is the undamaged baseline state
        - $\text{lor}_{ch}(t) = \text{lor}_{ch}(ce(t), D_{state}(t))$ (damaged ecosystems typically less resilient)
      - **Implementation Note**: These feedbacks often require numerical simulation rather than analytical solutions.
    - **Inter-ecosystem Linkages**: Vulnerability of ecosystem $ce_j$ depends on the state of ecosystem $ce_i$.
      - Let $D_{state,i}$ be the damage state of $ce_i$.
      - Exposure for $ce_j$: $e_{ch,j} = \mathcal{F}_{link,E}(ce_i, D_{state,i}, \dots)$ (e.g., damaged reef $ce_i$ increases wave exposure for seagrass $ce_j$).
      - Lack of resilience for $ce_j$: $\text{lor}_{ch,j} = \mathcal{F}_{link,R}(ce_i, D_{state,i}, \dots)$ (e.g., damaged mangroves $ce_i$ reduce sediment supply for marsh $ce_j$, affecting its recovery).
      - Then $CV(ce_j | ce_i, D_{state,i}) = \mathbb{E}[D(ce_j, e_{ch,j}(\cdot), \text{lor}_{ch,j}(\cdot))]$.
      - This can lead to a system of coupled CV equations for a network of ecosystems $\mathbf{CE} = \{ce_1, \dots, ce_N\}$.
- **Notes**:
  - These feedbacks can significantly increase model complexity, often requiring numerical simulation.
  - $\mathcal{F}_{EH}$, $Rec$, $\mathcal{F}_{link,E}$, $\mathcal{F}_{link,R}$ are complex functions/models specific to the ecosystems and hazards.

#### 8. **Uncertainty Quantification**
- **Definition**: Systematic assessment of uncertainty in $CV(ce)$ arising from uncertainties in input parameters, model structure, and hazard probabilities.
- **Mathematical Representation**:
    - Let $\Theta = \{\theta_1, \theta_2, \dots, \theta_M\}$ be the set of all uncertain parameters and model choices (e.g., parameters in $p(h)$, weights $w_j$ in $\text{lor}_{ch}$, parameters in $f(h)$ and $g(\text{lor}_{ch})$, choice of functional forms).
    - Each $\theta_k$ has an associated probability distribution $P(\theta_k)$ or a set of alternative models.
    - Coastal Vulnerability becomes a random variable conditional on $\Theta$: $CV(ce | \Theta)$.
    - The goal is to find the probability distribution of $CV(ce)$, denoted $P(CV(ce))$.
    - **Methods**:
        - **Monte Carlo Simulation**:
            1. Sample $N$ sets of parameters $\Theta^{(s)}$ from their respective distributions $P(\Theta_k)$.
            2. For each sample $s=1, \dots, N$, calculate $CV^{(s)}(ce) = \mathbb{E}[D(ce, e_{ch}(\Theta^{(s)}), \text{lor}_{ch}(\Theta^{(s)})) | \Theta^{(s)}]$.
            3. The ensemble $\{CV^{(s)}(ce)\}$ approximates $P(CV(ce))$.
        - **Sensitivity Analysis**: Evaluate $\frac{\partial CV}{\partial \theta_k}$ to identify key sources of uncertainty.
- **Output**:
  - Not just a point estimate for $CV(ce)$, but also:
    - Mean: $\mathbb{E}_{\Theta}[CV(ce | \Theta)]$
    - Variance: $Var_{\Theta}[CV(ce | \Theta)]$
    - Confidence Intervals / Credible Intervals (e.g., 5th-95th percentiles).
    - Probability Density Function (PDF) or Cumulative Distribution Function (CDF) of $CV(ce)$.
- **Notes**:
  - Crucial for decision-making, as it provides a measure of confidence in the $CV$ estimate.

#### 9. **Normalization and Aggregation for Indices**
- **Definition**: Standardizing $CV(ce)$ values for comparison across different ecosystems or damage units, and combining them into composite indices for broader assessment.
- **Mathematical Representation**:
    - **Normalization**: Transforming $CV(ce)$ to a common scale, typically [0, 1].
      - **Method 1 (Min-Max Scaling)** - Use for relative comparisons:
        $$ CV_{norm}(ce) = \frac{CV(ce) - CV_{min}}{CV_{max} - CV_{min}} $$
        Where $CV_{min}$ and $CV_{max}$ are minimum/maximum values across comparison set.
        - **Limitation**: Highly sensitive to outliers; single extreme value compresses all other rankings
        - **Recommendation**: Remove outliers or use robust percentile-based scaling (e.g., 5th-95th percentiles)
      - **Method 2 (Maximum Damage Scaling)** - Use for absolute vulnerability:
        $$ CV_{norm}(ce) = \frac{CV(ce)}{\phi(ce)} $$
        Where $\phi(ce)$ is the maximum possible damage. Represents proportion of ecosystem at risk.
      - **Method 3 (Z-Score Normalization)** - Alternative for outlier robustness:
        $$ CV_{norm}(ce) = \frac{CV(ce) - \mu_{CV}}{\sigma_{CV}} $$
        Where $\mu_{CV}$ and $\sigma_{CV}$ are mean and standard deviation of CV values.
      - **Selection Criteria**: Use Method 1 for ecosystem rankings (with outlier handling), Method 2 for absolute risk assessment, Method 3 when outliers are problematic.
    - **Aggregation**: Combining $CV$ values from multiple ecosystems ($ce_k, k=1, \dots, N_{eco}$) into a single index.
      - **Regional Vulnerability Index (RVI)**:
        $$ RVI = \sum_{k=1}^{N_{eco}} W_k \cdot CV_{norm}(ce_k) $$
        Where:
        - $W_k$: Weight assigned to ecosystem $ce_k$ determined via MCDA or expert elicitation
        - $\sum_{k=1}^{N_{eco}} W_k = 1$ for weighted average interpretation
        - **Weight Determination**: Use Analytic Hierarchy Process (AHP) or similar MCDA methods
        - **Mandatory Sensitivity Analysis**: Assess RVI robustness to weight variations ($W_k \pm \delta$)
- **Domain**:
  - $CV_{norm}(ce) \in [0, 1]$ (enforced by normalization)
  - $RVI \in [0, 1]$ (if $W_k$ sum to 1 and $CV_{norm} \in [0,1]$)
- **Consistency Checks**:
  - Sensitivity analysis on weight choices
  - Rank correlation analysis between different weighting schemes
- **Notes**:
  - Method selection significantly affects rankings - document rationale
  - Provide uncertainty bounds on aggregated indices

#### 10. **Adaptation and Management Interventions**
- **Definition**: Evaluating the potential of human actions to modify coastal vulnerability by altering exposure, resilience, or ecosystem characteristics.
- **Mathematical Representation**:
    - Let $\mathcal{M}$ be a set of possible management interventions $M_j \in \mathcal{M}$.
    - An intervention $M_j$ can modify:
      - Ecosystem characteristics: $ce \rightarrow ce'(M_j)$
      - Exposure: $e_{ch} \rightarrow e'_{ch}(M_j, ce')$ (e.g., building a seawall, restoring a reef)
      - Lack of Resilience: $\text{lor}_{ch} \rightarrow \text{lor}'_{ch}(M_j, ce')$ (e.g., habitat restoration improving $A(ce')$, $R(ce')$, $S(ce')$).
    - **Vulnerability with Intervention**:
      $$ CV(ce, M_j) = \mathbb{E}[D(ce'(M_j), e'_{ch}(M_j, ce'(M_j)), \text{lor}'_{ch}(M_j, ce'(M_j)))] $$
    - **Effectiveness of Intervention (Vulnerability Reduction)**:
      $$ \Delta CV(M_j) = CV_{baseline}(ce) - CV(ce, M_j) $$
      Where $CV_{baseline}(ce)$ is the vulnerability without intervention.
    - **Cost-Effectiveness Analysis**:
      - Let $C(M_j)$ be the cost of implementing intervention $M_j$.
      - Cost-Effectiveness Ratio: $CER(M_j) = \frac{C(M_j)}{\Delta CV(M_j)}$ (cost per unit of vulnerability reduced).
      - Benefit-Cost Ratio (if $\Delta CV$ can be monetized): $BCR(M_j) = \frac{\text{Monetized}(\Delta CV(M_j))}{C(M_j)}$.
- **Optimization**:
  - Find $M_j^* \in \mathcal{M}$ that maximizes $\Delta CV(M_j)$ subject to a budget constraint $C(M_j) \leq B_{max}$.
  - Or, find $M_j^*$ that minimizes $CER(M_j)$.
- **Notes**:
  - This formalization allows for a quantitative comparison of different adaptation strategies.
  - Modeling the effect of $M_j$ on $ce, e_{ch}, \text{lor}_{ch}$ is a significant research and modeling challenge itself.
  - The framework can also be used to assess maladaptation, where $CV(ce, M_j) > CV_{baseline}(ce)$.
