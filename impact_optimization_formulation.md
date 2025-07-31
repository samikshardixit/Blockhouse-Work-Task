# 📘 Optimization Formulation to Minimize Temporary Market Impact

## 🎯 Objective

Given:
- **S**: Total number of shares to buy (known at start of day)
- **N = 390**: Number of trading periods (1-minute intervals)
- **gₜ(xₜ)**: Temporary impact (slippage) function for trading `xₜ` shares at time `t`

We want to minimize total slippage over the day by choosing an allocation vector:

$$\mathbf{x} = [x_1, x_2, \dots, x_N] \in \mathbb{R}_{\ge 0}^N $$

### Problem:

$$
\min_{\mathbf{x}} \sum_{t=1}^{N} g_t(x_t)
\quad \text{subject to} \quad \sum_{t=1}^{N} x_t = S
$$

---

## 📐 Modeling the Temporary Impact Function

From data, we model each impact function as a power law:


$$ g_t(x_t) = \alpha_t x_t^{\gamma_t} $$

- \( $ \alpha_t > 0 $ \): scales impact at time t  
- \( $ \gamma_t > 1 $ \): controls convexity (e.g., 1.1–1.5 typical)

---

## ✅ Final Optimization Problem

$$
\min_{\{x_t\}_{t=1}^{390}} \sum_{t=1}^{390} \alpha_t x_t^{\gamma_t}
\quad \text{subject to} \quad \sum_{t=1}^{390} x_t = S,\quad x_t \ge 0 \ \forall t
$$

---

## ✏️ Optional Constraints

- **Max Participation Rate**:
  
  $$ x_t \le \eta \cdot D_t$$ 

  where \( $ D_t $ \) is top-10 depth at time t, and \( $ \eta \in [0, 1] $ \)

- **Volatility Penalty**:
  $$
  + \lambda \sum_{t=1}^{390} \sigma_t x_t^2
  $$
  where \( $ \sigma_t $ \) is per-minute mid-price volatility

---

## 🧠 Solving Strategy

- Problem is **convex** if \( $ \gamma_t \ge 1 $ \)
- Use standard solvers: CVXPY, SciPy, or custom Lagrangian method
