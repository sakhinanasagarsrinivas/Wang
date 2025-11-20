\section*{EXPECTILE REGRESSION}

\textbf{Offline dataset.}  
We assume an offline transition dataset
\[
\mathcal{D} = \{(s,a,r,s')\}.
\]

\begin{itemize}
    \item $s \in \mathcal{S}$ : state
    \item $a \in \mathcal{A}$ : an action sampled from the offline dataset at state $s$
    \item $D_s$ : empirical distribution of dataset actions observed at state $s$
    \item $Q_\phi(s,a) \in \mathbb{R}$ : differentiable Q-network with parameters $\phi \in \mathbb{R}^d$
    \item $V_\psi(s) \in \mathbb{R}$ : differentiable value network with parameters $\psi \in \mathbb{R}^k$
\end{itemize}

\vspace{6pt}

Residual:
\[
\delta(s,a) = Q_\phi(s,a) - V_\psi(s).
\]

Indicator:
\[
\mathbf{1}(\delta < 0) =
\begin{cases}
1, & \delta < 0,\\
0, & \delta \ge 0.
\end{cases}
\]

\section*{Step 1 — Expectile Loss}

The expectile regression loss is
\[
L_\tau(y,\mu)
    = \bigl|\tau - \mathbf{1}(y < \mu)\bigr|\, (y-\mu)^2,
    \qquad \tau \in (0,1).
\]

In offline RL:
\[
y = Q_\phi(s,a),\qquad \mu = V_\psi(s).
\]

Thus,
\[
L_\tau(Q_\phi(s,a),V_\psi(s))
    = \bigl|\tau - \mathbf{1}(Q_\phi(s,a) < V_\psi(s))\bigr|\,
      (Q_\phi(s,a) - V_\psi(s))^2.
\]

\[
\mathbf{1 }\bigl(Q_\phi(s,a) < V_\psi(s)\bigr)
=
\begin{cases}
1, & Q_\phi(s,a) < V_\psi(s),\\[6pt]
0, & Q_\phi(s,a) \ge V_\psi(s).
\end{cases}
\]

% ============================================================
\subsection*{Case 1: $Q_\phi(s,a) > V_\psi(s)$ ($\delta>0$)}

Indicator:
\[
\mathbf{1}(\delta<0)=0.
\]

Loss:
\[
L_\tau = \tau\,\delta^2.
\]

\[
\boxed{
L_\tau = \tau\,(Q_\phi(s,a)-V_\psi(s))^2
}
\]

Gradient:
\[
\frac{\partial L_\tau}{\partial V_\psi(s)}
    = \tau \cdot 2\, (V_\psi(s)-Q_\phi(s,a)) < 0.
\]

Thus:
\[
\boxed{
V_\psi(s)\ \text{is pushed upward}.
}
\]

% ============================================================
\subsection*{Case 2: $Q_\phi(s,a) < V_\psi(s)$ ($\delta<0$)}

Indicator:
\[
\mathbf{1}(\delta<0)=1.
\]

Loss:
\[
L_\tau = (1-\tau)\,\delta^2.
\]

\[
\boxed{
L_\tau = (1-\tau)\,(Q_\phi(s,a)-V_\psi(s))^2
}
\]

Gradient:
\[
\frac{\partial L_\tau}{\partial V_\psi(s)}
    = (1-\tau)\cdot 2\, (V_\psi(s)-Q_\phi(s,a)) > 0.
\]

Thus:
\[
\boxed{
V_\psi(s)\ \text{is pushed downward}.
}
\]

% ============================================================
\subsection*{Case 3: $Q_\phi(s,a) = V_\psi(s)$ ($\delta=0$)}

\[
L_\tau = 0, \qquad
\frac{\partial L_\tau}{\partial V_\psi(s)} = 0.
\]

\[
\boxed{
\text{No update.}
}
\]

% ============================================================
\section*{Summary Table}

Let $\delta = Q_\phi(s,a) - V_\psi(s)$.

\begin{center}
\begin{tabular}{c|c|c|c|c|c}
\textbf{Case} & \textbf{Condition} & \textbf{Indicator} &
\textbf{Loss} & \textbf{Grad Sign} &
\textbf{Effect on $V_\psi(s)$} \\
\hline
1 & $\delta>0$ & $0$ &
$\tau\delta^2$ & $<0$ & $V_\psi(s)\uparrow$\\
\hline
2 & $\delta<0$ & $1$ &
$(1-\tau)\delta^2$ & $>0$ & $V_\psi(s)\downarrow$\\
\hline
3 & $\delta=0$ & — &
$0$ & $0$ & no change
\end{tabular}
\end{center}

\section*{Interpretation}

Upward weight $=\tau$, downward weight $=1-\tau$.  
If $\tau>0.5$, upward corrections dominate, and $V_\psi(s)$ tracks the \emph{upper expectile} of the Q-values.

\section*{Value Network Expectile Regression Objective}

\[
V_\psi(s)
    = \arg\min_{v \in \mathbb{R}}
      \mathbb{E}_{a \sim D_s}
      \left[L_\tau(Q_\phi(s,a),v)\right].
\]

\newpage
\pagebreak

\section*{Implicit Q-Learning (IQL)}
\section*{0. Notation (Short, Complete)}
\[
\mathcal{D}=\{(s_t,a_t,r_{t+1},s_{t+1})\}.
\]
\[
\begin{aligned}
&Q_\phi(s_t,a_t) &&\text{Q-network} \\
&V_\psi(s_t) &&\text{Value network} \\
&\pi_\theta(a_t\mid s_t) &&\text{Policy (discrete or continuous)} \\
&D_{s_t}(a) &&\text{Empirical action distribution at } s_t \\
&\delta_t = Q_\phi(s_t,a_t)-V_\psi(s_t) &&\text{Residual} \\
&A_{\phi,\psi}(s_t,a_t)=\delta_t &&\text{Implicit advantage} \\
&w(s_t,a_t)=\exp(A_{\phi,\psi}(s_t,a_t)/\beta) &&\text{Advantage weight} \\
&L_\tau(y,\mu) &&\text{Expectile loss} \\
&L_Q(\phi),\; L_V(\psi),\; J_{\text{actor}}(\theta) &&\text{Loss functions} \\
&\tau\in(0,1),\;\beta>0,\;\gamma\in(0,1) &&\text{Hyperparameters}
\end{aligned}
\]
TD target:
\[
y^{TD}_t = r_{t+1} + \gamma V_\psi(s_{t+1}).
\]
\section*{1. Value Function via Expectile Regression}
\[
L_\tau(y,\mu)=\left|\tau-\mathbf{1}(y<\mu)\right|(y-\mu)^2.
\]
\[
L_\tau(Q_\phi(s_t,a_t),V_\psi(s_t))
=
\left|\tau-\mathbf{1}\left(Q_\phi(s_t,a_t)<V_\psi(s_t)\right)\right|
\left(Q_\phi(s_t,a_t)-V_\psi(s_t)\right)^2.
\]
\[
V_\psi(s_t)
=
\arg\min_{v\in\mathbb{R}}
\mathbb{E}_{a\sim D_{s_t}}
\left[L_\tau(Q_\phi(s_t,a),v)\right].
\]
\section*{2. Implicit Advantage}
\[
A_{\phi,\psi}(s_t,a_t)
=
Q_\phi(s_t,a_t)-V_\psi(s_t).
\]
\section*{3. Policy Models}
\subsection*{Discrete (Softmax)}
\[
\pi_\theta(a_t\mid s_t)
=
\frac{\exp(f_\theta(s_t,a_t))}
{\sum_{a'\in\mathcal{A}}\exp(f_\theta(s_t,a'))}.
\]
\subsection*{Continuous (Gaussian)}
\[
\pi_\theta(a_t\mid s_t)
=
\mathcal{N}\!\left(a_t \mid \mu_\theta(s_t),\Sigma_\theta(s_t)\right).
\]
\section*{4. Advantage-Weighted Actor Learning}
\[
w(s_t,a_t)=\exp\!\left(\frac{A_{\phi,\psi}(s_t,a_t)}{\beta}\right).
\]
\[
J_{\text{actor}}(\theta)
=
\mathbb{E}_{(s_t,a_t)\sim\mathcal{D}}
\left[w(s_t,a_t)\log\pi_\theta(a_t\mid s_t)\right].
\]
\section*{5. Q-Function Learning}
\[
y^{TD}_t=r_{t+1}+\gamma V_\psi(s_{t+1}).
\]
\[
L_Q(\phi)=
\mathbb{E}\left[
\left(Q_\phi(s_t,a_t)-y^{TD}_t\right)^2
\right].
\]
\section*{6. Parameter Updates}
\[
\phi \leftarrow \phi - \eta_\phi \nabla_\phi L_Q(\phi),
\]
\[
\psi \leftarrow \psi - \eta_\psi
\nabla_\psi
\mathbb{E}_{a\sim D_{s_t}}
\left[L_\tau(Q_\phi(s_t,a),V_\psi(s_t))\right],
\]
\[
\theta \leftarrow 
\theta + \eta_\theta
\nabla_\theta J_{\text{actor}}(\theta).
\]
\section*{7. Compact Objective Summary}
\[
\boxed{
\begin{aligned}
&L_Q(\phi)=
\mathbb{E}\!\left[
\left(Q_\phi(s_t,a_t)-\left(r_{t+1}+\gamma V_\psi(s_{t+1})\right)\right)^2
\right]
\\[6pt]
&L_V(\psi)=
\mathbb{E}\!\left[
\left|\tau-\mathbf{1}\left(Q_\phi(s_t,a_t)<V_\psi(s_t)\right)\right|
\left(Q_\phi(s_t,a_t)-V_\psi(s_t)\right)^2
\right]
\\[6pt]
&J_{\text{actor}}(\theta)=
\mathbb{E}\!\left[
\exp\!\left(\frac{Q_\phi(s_t,a_t)-V_\psi(s_t)}{\beta}\right)
\log\pi_\theta(a_t\mid s_t)
\right]
\end{aligned}}
\]
\section*{8. Short Explanation}
IQL is an offline RL method where:
\subsection*{1. The value function is fitted using expectile regression}
\[
V_\psi(s_t)
=
\arg\min_{v}
\mathbb{E}_{a\sim D_{s_t}}
\left[
\left|\tau-\mathbf{1}\left(Q_\phi(s_t,a)<v\right)\right|
\left(Q_\phi(s_t,a)-v\right)^2
\right].
\]
\subsection*{2. The Q-function is fitted using TD(0)}
\[
L_Q(\phi)=
\mathbb{E}\left[
\left(Q_\phi(s_t,a_t)-(r_{t+1}+\gamma V_\psi(s_{t+1}))\right)^2
\right].
\]
\subsection*{3. The policy is learned by advantage-weighted imitation}
\[
J_{\text{actor}}(\theta)=
\mathbb{E}\left[
\exp\!\left(\frac{Q_\phi(s_t,a_t)-V_\psi(s_t)}{\beta}\right)
\log\pi_\theta(a_t\mid s_t)
\right].
\]
\subsection*{4. All expectations are over dataset actions only}
\[
(s_t,a_t)\sim\mathcal{D}, \qquad a\sim D_{s_t}.
\]
Thus,
\[
\text{IQL avoids OOD error because the value network is fitted only on dataset actions}.
\]

\begin{algorithm}[H]
\caption{Implicit Q-Learning (IQL)}
\begin{algorithmic}[1]

\State \textbf{Input:} Offline dataset $\mathcal{D}=\{(s_t,a_t,r_{t+1},s_{t+1})\}$

\State \textbf{Initialize:} 
Q-network $Q_\phi$, 
value network $V_\psi$, 
policy $\pi_\theta$,
learning rates $\eta_\phi,\,\eta_\psi,\,\eta_\theta$,
discount $\gamma$,
expectile $\tau$,
actor temperature $\beta$.

\While{training}

    \State Sample minibatch $B$ from $\mathcal{D}$.

    \ForAll{$(s_t,a_t,r_{t+1},s_{t+1}) \in B$}

        \State \textbf{// TD(0) target}
        \State $y^{TD}_t = r_{t+1} + \gamma\, V_\psi(s_{t+1})$

        \State \textbf{// Q-function loss}
        \State $L_Q = (Q_\phi(s_t,a_t) - y^{TD}_t)^2$

        \State \textbf{// Expectile value loss}
        \State Compute residual: $\delta_t = Q_\phi(s_t,a_t) - V_\psi(s_t)$
        \State Compute weight: $u_t = |\tau - \mathbf{1}(\delta_t < 0)|$
        \State $L_V = u_t \cdot \delta_t^2$

        \State \textbf{// Implicit advantage}
        \State $A_t = Q_\phi(s_t,a_t) - V_\psi(s_t)$

        \State \textbf{// Advantage weight}
        \State $w_t = \exp(A_t / \beta)$

        \State \textbf{// Actor loss (advantage-weighted)}
        \State $L_{\text{actor}} = - w_t \cdot \log \pi_\theta(a_t \mid s_t)$

    \EndFor

    \State \textbf{// Update networks}
    \State $\phi \gets \phi - \eta_\phi \nabla_\phi L_Q$
    \State $\psi \gets \psi - \eta_\psi \nabla_\psi L_V$
    \State $\theta \gets \theta - \eta_\theta \nabla_\theta L_{\text{actor}}$

\EndWhile

\end{algorithmic}
\end{algorithm}


\newpage
\pagebreak

\section*{Conservative Q-Learning (CQL)}

\section*{0. Notation (Short and Complete)}

\[
\mathcal{D}=\{(s_t,a_t,r_{t+1},s_{t+1},d_{t+1})\}.
\]

\[
\begin{aligned}
&Q_{\phi_1}(s,a),\,Q_{\phi_2}(s,a)
&&\text{Twin Q-networks (SAC-style)},\\
&Q_{\phi^-_1}(s,a),\,Q_{\phi^-_2}(s,a)
&&\text{Target twin Q-networks (Polyak averaged)},\\
&\pi_\theta(a\mid s)
&&\text{Stochastic policy (Gaussian for continuous; categorical for discrete)},\\
&\mathcal{A}
&&\text{Action space (finite or continuous)},\\
&\gamma\in(0,1)
&&\text{Discount factor},\\
&d_{t+1}\in\{0,1\}
&&\text{Terminal flag},\\
&\alpha>0
&&\text{CQL regularization weight},\\
&\alpha_{\text{ent}}>0
&&\text{Entropy temperature (for policy)},\\
&L_{\text{Bellman}}, L_{\text{CQL}}, L_{\text{total}}
&&\text{Loss terms}.
\end{aligned}
\]

\section*{1. Bellman Regression (Twin Critic Form)}

\subsection*{TD(0) Target (SAC-Style)}

\[
y^{TD}_t
=
r_{t+1}
+
\gamma (1-d_{t+1})
\left[
\min_{i=1,2}
Q_{\phi^-_i}(s_{t+1}, a'_{t+1})
-
\alpha_{\text{ent}} \log \pi_\theta(a'_{t+1} \mid s_{t+1})
\right],
\]
\[
a'_{t+1} \sim \pi_\theta(\cdot \mid s_{t+1}).
\]

\subsection*{Twin Critic Bellman Error}

\[
L_{\text{Bellman}}
=
\sum_{i=1}^2\;
\mathbb{E}_{(s_t,a_t)\sim\mathcal{D}}
\left[
\left(
Q_{\phi_i}(s_t,a_t)-y^{TD}_t
\right)^2
\right].
\]

\section*{2. Conservative Penalty (Core of CQL)}

CQL modifies only one critic, typically $Q_{\phi_1}$, to push down Q-values for OOD actions and increase Q-values for dataset actions.

\subsection*{2.1 Discrete Action Spaces}

\[
L_{\text{CQL}}^{\text{disc}}
=
\mathbb{E}_{s_t\sim\mathcal{D}}
\left[
\log
\sum_{a \in \mathcal{A}}
\exp(Q_{\phi_1}(s_t,a))
-
Q_{\phi_1}(s_t,a_t)
\right].
\]

\subsection*{2.2 Continuous Action Spaces}

\[
a^{(i)} \sim q(a \mid s_t),
\]

\[
q(a\mid s)
= 
\lambda_1 q_{\text{uniform}}(a)
+ 
\lambda_2 q_{\text{random}}(a)
+
\lambda_3 \pi_\theta(a \mid s),
\qquad
\sum \lambda_i = 1,\;
\lambda_i \ge 0.
\]

\[
L_{\text{CQL}}^{\text{cont}}
=
\mathbb{E}_{s_t\sim\mathcal{D}}
\left[
\log
\left(
\frac{1}{M}
\sum_{i=1}^{M}
\frac{\exp(Q_{\phi_1}(s_t,a^{(i)}))}{q(a^{(i)}\mid s_t)}
\right)
-
Q_{\phi_1}(s_t,a_t)
\right].
\]

\subsection*{Simplified Practical Form}

\[
L_{\text{CQL}}^{\text{cont-simple}}
=
\mathbb{E}_{s_t\sim\mathcal{D}}
\left[
\log
\left(
\frac{1}{M}
\sum_{i=1}^{M}
\exp(Q_{\phi_1}(s_t,a^{(i)}))
\right)
-
Q_{\phi_1}(s_t,a_t)
\right].
\]

\section*{3. Full CQL Critic Objective}

\[
L_{\text{total}}
=
L_{\text{Bellman}}
+
\alpha L_{\text{CQL}},
\]

\[
L_{\text{CQL}} =
\begin{cases}
L_{\text{CQL}}^{\text{disc}}, & \text{if discrete},\\[4pt]
L_{\text{CQL}}^{\text{cont-simple}} \text{ or } L_{\text{CQL}}^{\text{cont}}, & \text{if continuous}.
\end{cases}
\]

\[
Q(s,a_{\text{OOD}}) \text{ decreases},
\qquad
Q(s,a_{t}) \text{ increases}.
\]

\section*{4. Policy Learning (Optional: CQL-SAC)}

\[
J_{\text{actor}}(\theta)
=
\mathbb{E}_{s_t\sim\mathcal{D},\, a\sim\pi_\theta(\cdot\mid s_t)}
\left[
\alpha_{\text{ent}}\log\pi_\theta(a\mid s_t)
-
Q_{\phi_1}(s_t,a)
\right].
\]

\[
\pi_\theta^*
=
\arg\min_{\pi_\theta}
\mathrm{KL}
\left(
\pi_\theta(\cdot\mid s)
\;\Big\|\;
\frac{\exp(Q_{\phi_1}(s,a)/\alpha_{\text{ent}})}{Z(s)}
\right),
\]

\[
Z(s)=\int_{\mathcal{A}}\exp\left(Q_{\phi_1}(s,a)/\alpha_{\text{ent}}\right) da.
\]

\section*{5. Parameter Updates}

\[
\phi_i \leftarrow \phi_i - \eta_\phi \nabla_{\phi_i} L_{\text{total}}, 
\qquad i = 1,2.
\]

\[
\theta \leftarrow \theta - \eta_\theta \nabla_\theta J_{\text{actor}}.
\]

\[
\phi^-_i \leftarrow \tau\,\phi_i + (1-\tau)\phi^-_i,
\qquad i = 1,2.
\]

\section*{6. Compact Objective Summary}

\[
\boxed{
\begin{aligned}
&\textbf{Bellman Loss:}\quad
L_{\text{Bellman}}
=
\sum_{i=1}^2
\mathbb{E}
\left[
(Q_{\phi_i}(s_t,a_t)-y^{TD}_t)^2
\right],
\\[6pt]
&\textbf{CQL Loss (disc):}\quad
L_{\text{CQL}}
=
\mathbb{E}
\left[
\log\sum_a \exp(Q_{\phi_1}(s_t,a))
-
Q_{\phi_1}(s_t,a_t)
\right],
\\[6pt]
&\textbf{CQL Loss (cont):}\quad
L_{\text{CQL}}
=
\mathbb{E}
\left[
\log \tfrac{1}{M}\sum_i \exp(Q_{\phi_1}(s_t,a^{(i)}))
-
Q_{\phi_1}(s_t,a_t)
\right],
\\[6pt]
&\textbf{Total Loss:}\quad
L_{\text{total}}
=
L_{\text{Bellman}}
+
\alpha L_{\text{CQL}},
\\[6pt]
&\textbf{Actor Loss (CQL-SAC):}\quad
J_{\text{actor}}
=
\mathbb{E}
\left[
\alpha_{\text{ent}}\log\pi_\theta(a\mid s_t)
-
Q_{\phi_1}(s_t,a)
\right].
\end{aligned}}
\]


\begin{algorithm}[H]
\caption{Conservative Q-Learning (CQL)}
\begin{algorithmic}[1]

\State \textbf{Input:} Offline dataset $\mathcal{D}=\{(s_t,a_t,r_{t+1},s_{t+1},d_{t+1})\}$
\State \textbf{Initialize:} Q-networks $Q_{\phi_1}, Q_{\phi_2}$; target networks $Q_{\phi^-_1}, Q_{\phi^-_2}$; 
policy $\pi_\theta$ (optional); sampler $q(a\mid s)$;
learning rates $\eta_\phi, \eta_\theta$; discount $\gamma$; CQL weight $\alpha$; entropy temperature $\alpha_{\text{ent}}$.

\While{training}

    \State Sample minibatch $B$ from $\mathcal{D}$.

    \ForAll{$(s_t,a_t,r_{t+1},s_{t+1},d_{t+1}) \in B$}

        \State Sample next action $a'_{t+1} \sim \pi_\theta(\cdot\mid s_{t+1})$

        \State \textbf{// SAC TD target}
        \State $y^{TD}_t = r_{t+1} + \gamma (1-d_{t+1}) \left( \min(Q_{\phi^-_1}(s_{t+1},a'_{t+1}),\, Q_{\phi^-_2}(s_{t+1},a'_{t+1})) - \alpha_{\text{ent}}\log\pi_\theta(a'_{t+1}\mid s_{t+1}) \right)$

        \State \textbf{// Bellman critic loss}
        \State $L_{\text{Bellman}} = (Q_{\phi_1}(s_t,a_t)-y^{TD}_t)^2 + (Q_{\phi_2}(s_t,a_t)-y^{TD}_t)^2$

        \State \textbf{// CQL penalty}
        \If{discrete action space}
            \State $L_{\text{CQL}} = \log\left( \sum_{a\in\mathcal{A}} \exp(Q_{\phi_1}(s_t,a)) \right) - Q_{\phi_1}(s_t,a_t)$
        \Else
            \State Sample $M$ actions $a^{(i)} \sim q(a\mid s_t)$
            \State $L_{\text{CQL}} = \log\left( \frac{1}{M}\sum_{i=1}^{M} \exp(Q_{\phi_1}(s_t,a^{(i)})) \right) - Q_{\phi_1}(s_t,a_t)$
        \EndIf

        \State $L_{\text{total}} = L_{\text{Bellman}} + \alpha\,L_{\text{CQL}}$

    \EndFor

    \State \textbf{// Update critics}
    \State $\phi_i \gets \phi_i - \eta_\phi \nabla_{\phi_i} L_{\text{total}}$ \quad for $i=1,2$

    \If{policy learning enabled}
        \State \textbf{// Actor update}
        \State $J_{\text{actor}} = \mathbb{E}_{s\sim\mathcal{D},\,a\sim \pi_\theta} \left[ \alpha_{\text{ent}}\log\pi_\theta(a\mid s) - Q_{\phi_1}(s,a) \right]$
        \State $\theta \gets \theta - \eta_\theta \nabla_\theta J_{\text{actor}}$
    \EndIf

    \State \textbf{// Polyak averaging}
    \State $\phi^-_i \gets \tau\,\phi_i + (1-\tau)\phi^-_i$ \quad for $i=1,2$

\EndWhile

\end{algorithmic}
\end{algorithm}



\newpage
\pagebreak


\section*{Batched Constrained Q-Learning (BCQL)}

\section*{0. Notation (Short, Complete)}

\[
\mathcal{D}=\{(s_t,a_t,r_{t+1},s_{t+1})\}.
\]

\[
\begin{aligned}
&Q_\phi(s,a) &&\text{Q-network with parameters }\phi,\\
&\mathcal{A}_{\mathcal{D}}(s) &&\text{Dataset-supported actions at state } s, \\
&\mathcal{A} &&\text{Action space (finite)},\\
&\gamma\in(0,1) &&\text{Discount factor},\\
&L_Q(\phi) &&\text{BCQL loss},\\
&y^{BCQL}_t &&\text{BCQL TD target}.
\end{aligned}
\]

\[
\mathcal{A}_{\mathcal{D}}(s)
=
\{\,a \mid (s,a) \text{ appears in the dataset}\,\}.
\]

\section*{1. Bellman Target (Constrained Maximization)}

\[
y^{BCQL}_t
=
r_{t+1}
+
\gamma
\max_{a' \in \mathcal{A}_{\mathcal{D}}(s_{t+1})}
Q_\phi(s_{t+1},a').
\]

\subsection*{Key principle:}

\[
\max_{a'\in\mathcal{A}}
\quad\Longrightarrow\quad
\max_{a'\in\mathcal{A}_{\mathcal{D}}(s)}.
\]

\section*{2. BCQL Loss}

\[
L_Q(\phi)
=
\mathbb{E}_{(s_t,a_t,r_{t+1},s_{t+1})\sim\mathcal{D}}
\left[
\left(
Q_\phi(s_t,a_t)
-
y^{BCQL}_t
\right)^2
\right].
\]

\section*{3. Dataset-Supported Action Set}

For discrete action spaces:

\[
\mathcal{A}_{\mathcal{D}}(s)
=
\{a\in\mathcal{A}\mid (s,a)\in\mathcal{D}\}.
\]

If a state $s$ appears $K$ times in the dataset with actions
\[
a^{(1)},a^{(2)},\dots,a^{(K)},
\]
then
\[
\mathcal{A}_{\mathcal{D}}(s)=\{a^{(1)},\dots,a^{(K)}\}.
\]

\section*{4. Comparison to Standard Q-Learning}

Standard Q-learning target:
\[
y^{QL}_t = r_{t+1}+\gamma\max_{a'\in\mathcal{A}}Q(s_{t+1},a').
\]

BCQL target:
\[
y^{BCQL}_t = r_{t+1}+\gamma\max_{a'\in\mathcal{A}_{\mathcal{D}}(s_{t+1})}Q(s_{t+1},a').
\]

\[
\text{BCQL is a constrained variant of Q-learning.}
\]

\section*{5. Parameter Update}

\[
\phi \leftarrow \phi - \eta_\phi \nabla_\phi L_Q(\phi).
\]

\section*{6. Compact Objective Summary}

\[
\boxed{
\begin{aligned}
&\mathcal{A}_{\mathcal{D}}(s)
=
\{a\mid (s,a)\in\mathcal{D}\},
\\[6pt]
&y^{BCQL}_t
=
r_{t+1}
+
\gamma
\max_{a'\in\mathcal{A}_{\mathcal{D}}(s_{t+1})}
Q_\phi(s_{t+1},a'),
\\[6pt]
&L_Q(\phi)
=
\mathbb{E}
\left[
\left(
Q_\phi(s_t,a_t)
-
y^{BCQL}_t
\right)^2
\right].
\end{aligned}}
\]

\section*{7. Short Explanation}

\subsection*{1. Offline Q-learning usually uses}
\[
\max_{a'} Q(s,a').
\]

\subsection*{2. BCQL restricts the backup to dataset actions only}
\[
a'\in\mathcal{A}_{\mathcal{D}}(s_{t+1}),
\]
so no extrapolation error occurs.

\subsection*{3. BCQL is a conservative offline RL method}
It never evaluates Q-values of unseen actions, which makes it stable but overly conservative.

\begin{algorithm}[H]
\caption{Batched Constrained Q-Learning (BCQL)}
\begin{algorithmic}[1]

\State \textbf{Input:} Offline dataset $\mathcal{D}=\{(s_t,a_t,r_{t+1},s_{t+1})\}$
\State \textbf{Initialize:} Q-network $Q_\phi$, learning rate $\eta_\phi$, discount $\gamma$

\While{training}

    \State Sample minibatch $B$ from $\mathcal{D}$

    \ForAll{$(s_t,a_t,r_{t+1},s_{t+1}) \in B$}

        \State \textbf{// Identify dataset-supported actions at $s_{t+1}$}
        \[
        \mathcal{A}_{\mathcal{D}}(s_{t+1})
        =
        \{\, a' \mid (s_{t+1},a') \in \mathcal{D} \,\}
        \]

        \State \textbf{// BCQL TD target}
        \[
        y^{BCQL}_t
        =
        r_{t+1}
        +
        \gamma
        \max_{a' \in \mathcal{A}_{\mathcal{D}}(s_{t+1})}
        Q_\phi(s_{t+1},a')
        \]

        \State \textbf{// Bellman error}
        \[
        L_Q
        =
        \left(
        Q_\phi(s_t,a_t)
        -
        y^{BCQL}_t
        \right)^2
        \]

    \EndFor

    \State \textbf{// Gradient step}
    \[
    \phi \leftarrow \phi - \eta_\phi \nabla_\phi L_Q
    \]

\EndWhile

\end{algorithmic}
\end{algorithm}

\newpage
\pagebreak
