.. _time_series_approach-ou_model:

.. note::
   The following implementations and documentation closely follow the publication by Zhengqin Zeng & Chi-Guhn Lee:
   `Pairs trading: optimal thresholds and profitability. Quantitative Finance, 14(11): 1881–1893
   <https://www.tandfonline.com/doi/pdf/10.1080/14697688.2014.917806>`_.

========================================
OU Model Optimal Trading Thresholds Zeng
========================================

In this paper, the authors enhance the work in `Bertram(2010)
<http://www.stagirit.org/sites/default/files/articles/a_0340_ssrn-id1505073.pdf>`_, which assumes no short selling
of the synthetic asset when finding the optimal trading thresholds. To resolve the assumption, they derive
a polynomial expression for the expectation of the first-passage time of an O-U process with two-sided boundary.
Then they simplify the problem of optimizing the expected return per unit of time for choosing optimal trading
thresholds to an equation solving problem.

.. warning::

    We use :math:`\theta` for long-term mean, :math:`\mu` for mean-reversion speed and :math:`\sigma` for amplitude
    of randomness of the O-U process, which is different from the reference paper.

Assumptions
###########

Price of the Traded Security
****************************

It models the price of the traded security :math:`p_t` as,

.. math::
    {p_t = e^{X_t}};\quad{X_{t_0} = x_0}

where :math:`X_t` satisfies the following stochastic differential equation,

.. math::
    {dX_t = {\mu}({\theta} - X_t)dt + {\sigma}dW_t}

where :math:`\theta` is the long-term mean, :math:`\mu` is the speed at which the values will regroup around the
long-term mean and :math:`\sigma` is the amplitude of randomness of the O-U process.

Trading Strategy
****************

The Trading strategy is defined as: 

.. math::
    \left\{
    \begin{array}{**lr**} 
    Open\ a\ short\ trade\ when\ Y_t = a_d\ and\ close\ the\ exiting\ short\ trade\ at\ Y_t = b_d.\\
    Open\ a\ long\ trade\ when\ Y_t = -a_d\ and\ close\ the\ exiting\ long\ trade\ at\ Y_t = -b_d.\\
    \end{array}
    \right.

where :math:`Y_t` is a dimensionless series transformed from the original time series :math:`X_t`, 

:math:`a_d` and :math:`b_d` is the entry and exit thresholds in the dimensionless system, respectively.

Trading Cycle
*************

The trading cycle is completed as :math:`Y_t` change from :math:`a_d` to :math:`b_d`, then back to :math:`a_d` or
:math:`-a_d`, and the trade length :math:`T` is defined as the time needed to complete a trading cycle.

Analytic Formulae
#################

Mean and Variance of the Trade Length
*************************************

.. math::
    E[T] = \frac{1}{2\mu}\sum_{k=0}^{\infty} \Gamma(\frac{2k + 1}{2})((\sqrt{2}a_d)^{2k + 1} - (\sqrt{2}b_d)^{2k + 1})/ (2k + 1)!,

.. math::
    V[T] = \frac{1}{\mu^2}(V[T_{a_d,\ b_d}] + V[T_{-a_d,\ a_d,\ b_d}]),

where

:math:`V[T_{a_d,\ b_d}]` is the variance of the time taken for the O-U process to travel from :math:`a_d` to :math:`b_d`,

:math:`V[T_{-a_d,\ a_d,\ b_d}]` is the variance of the time taken for the O-U process to travel from :math:`b_d` to :math:`a_d` or -:math:`a_d`.

.. math::
    V[T_{a_d,\ b_d}] = {w_1(a_d)} - {w_1(b_d)} - {w_2(a_d)} + {w_2(b_d)},

where 

:math:`w_1(z) = (\frac{1}{2} \sum_{k=1}^{\infty} \Gamma(\frac{k}{2}) (\sqrt{2}z)^k / k! )^2 - (\frac{1}{2} \sum_{n=1}^{\infty} (-1)^k \Gamma(\frac{k}{2}) (\sqrt{2}z)^k / k! )^2`,

:math:`w_2(z) = \sum_{k=1}^{\infty} \Gamma(\frac{2k - 1}{2}) \Psi(\frac{2k - 1}{2}) (\sqrt{2}z)^{2k - 1} / (2k - 1)!`.


.. math::
    V[T_{-a_d,\ a_d,\ b_d}] = E[T^{2}_{-a_d,\ a_d,\ b_d}] - E[T_{-a_d,\ a_d,\ b_d}]^2,

where

:math:`E[T_{-a_d,\ a_d,\ b_d}] = \frac{1}{2}\sum_{k=1}^{\infty} \Gamma(k)((\sqrt{2}a_d)^{2k} - (\sqrt{2}b_d)^{2k})/ (2k)!`,

:math:`E[T^{2}_{-a_d,\ a_d,\ b_d}] = e^{(b^2_d - a^2_d)/4}[g_1(a_d,\ b_d) - g_2(a_d,\ b_d)]`,

where

:math:`g_1(a_d,\ b_d) = [\frac{(m^{''}(\lambda,\ b_d)\ m(\lambda,\ a_d) - m^{'}(\lambda,\ a_d)\ m^{'}(\lambda,\ b_d))}{m^2(\lambda,\ a_d)}]|_{\lambda = 0}`,

:math:`g_2(a_d,\ b_d) =[\frac{(m^{''}(\lambda,\ a_d)\ m(\lambda,\ b_d) + m^{'}(\lambda,\ a_d)\ m^{'}(\lambda,\ b_d))}{m^2(\lambda,\ a_d)} - 2\frac{(m^{'}(\lambda,\ a_d))^2\ m(\lambda,\ b_d)}{m^3(\lambda,\ a_d)}]|_{\lambda = 0}`,

where :math:`m(\lambda, x) = D_{-\lambda}(x) + D_{-\lambda}(−x)`,

where :math:`D_{-\lambda}(x) = \sqrt{\frac{2}{\pi}} e^{x^2/4} \int_{0}^{\infty} t^{-\lambda} e^{-t^2/2} \cos(xt + \frac{\lambda\pi}{2})dt`.

Mean and Variance of the Return per Unit of Time
************************************************

.. math::
    \mu_s(a,\ b,\ c) = \frac{r(a,\ b,\ c)}{E [T]}

.. math::
    \sigma_s(a,\ b,\ c) = \frac{{r(a,\ b,\ c)}^2{V[T]}}{{E[T]}^3}

where :math:`r(a,\ b,\ c) = (|a − b| − c)` gives the continuously compound rate of return for a single trade
accounting for transaction cost,

where :math:`a`, :math:`b` denotes the entry and exit thresholds, respectively.

Optimal Strategies
##################

To calculate an optimal trading strategy, we seek to choose optimal entry and exit thresholds that maximise
the expected return per unit of time for a given transaction cost.

Get Optimal Thresholds by Maximizing the Expected Return
********************************************************

:math:`Case\ 1 \ \ 0 \leqslant b_d \leqslant a_d`

This paper shows that the maximum expected return occurs when :math:`b_d = 0`. Therefore, for a given transaction cost,
the following equation can be solved to find optimal :math:`a_d`.

.. math::
    \frac{1}{2}\sum_{k=0}^{\infty} \Gamma(\frac{2k + 1}{2})((\sqrt{2}a_d)^{2k + 1} / (2k + 1)! = (a - c) \frac{\sqrt{2}}{2}\sum_{k=0}^{\infty} \Gamma(\frac{2k}{2})((\sqrt{2}a_d)^{2k} / (2k + 1)!

:math:`Case\ 2 \ \ -a_d \leqslant b_d \leqslant 0`

This paper shows that the maximum expected return occurs when :math:`b_d = -a_d`. Therefore, for a given transaction cost,
the following equation can be solved to find optimal :math:`a_d`.

.. math::
    \frac{1}{2}\sum_{k=0}^{\infty} \Gamma(\frac{2k + 1}{2})((\sqrt{2}a_d)^{2k + 1} / (2k + 1)! = (a - \frac{c}{2}) \frac{\sqrt{2}}{2}\sum_{k=0}^{\infty} \Gamma(\frac{2k}{2})((\sqrt{2}a_d)^{2k} / (2k + 1)!

Back Transform from the Dimensionless System
********************************************

After calculating optimal thresholds in the dimensionless system, we need to use the following formula to transform them back to the original system.

.. math::
    k = k_d \frac{\sigma}{\sqrt{2\mu}} + \theta,

where :math:`k_d` = :math:`a_d`, :math:`b_d`, :math:`-a_d`, :math:`-b_d` and :math:`k` = :math:`a_s`, :math:`b_s`, :math:`a_l`, :math:`a_l`,

where

:math:`a_s`, :math:`b_s` denotes the entry and exit thresholds for a short position,

:math:`a_l`, :math:`b_l` denotes the entry and exit thresholds for a long position.


Implementation
##############

Initializing OU-Process Parameters
**********************************

One can initialize the O-U process by directly setting its parameters or by fitting the process to the given data.
The fitting method can refer to pp. 12-13 in the following book:
`Tim Leung and Xin Li, Optimal Mean reversion Trading: Mathematical Analysis and Practical Applications
<https://www.amazon.com/Optimal-Mean-Reversion-Trading-Mathematical/dp/9814725919>`_.

.. py:currentmodule:: arbitragelab.time_series_approach.ou_optimal_threshold_zeng

.. autoclass:: OUModelOptimalThresholdZeng
    :members: __init__

.. automethod:: OUModelOptimalThresholdZeng.construct_ou_model_from_given_parameters

.. automethod:: OUModelOptimalThresholdZeng.fit_ou_model_to_data

Getting Optimal Thresholds
**************************

This paper examines the problem of choosing an optimal strategy under two different cases. Case 1 corresponds to
the ‘Conventional Optimal Rule’, and case 2 corresponds to the ‘New Optimal Rule’. One can choose either to get the
thresholds. The following functions will return a tuple contains :math:`a_s`, :math:`b_s`, :math:`a_l` and :math:`a_l`,
where :math:`a_s`, :math:`b_s` denotes the entry and exit thresholds for a short position, :math:`a_l`, :math:`b_l`
denotes the entry and exit thresholds for a long position.

.. note::
    :code:`initial_guess` is used to speed up the process and ensure the target equation can be solved by
    :code:`scipy.optimize`. If the value of :code:`initial_guess` is not given, the default value will be
    :math:`(c + 10^{-2})\sqrt{2\mu} / \sigma`. From our experiment, the default value is suited for most of the cases.
    If you observe that the thresholds got by the functions is odd or the running time is larger than 5 second,
    please try a :code:`initial_guess` on different scales.

.. automethod:: OUModelOptimalThresholdZeng.get_threshold_by_conventional_optimal_rule

.. automethod:: OUModelOptimalThresholdZeng.get_threshold_by_new_optimal_rule

Calculating Metrics
*******************

One can calculate performance metrics for the trading strategy using the following functions.

.. automethod:: OUModelOptimalThresholdZeng.expected_trade_length

.. automethod:: OUModelOptimalThresholdZeng.trade_length_variance

.. automethod:: OUModelOptimalThresholdZeng.expected_return

.. automethod:: OUModelOptimalThresholdZeng.return_variance

.. automethod:: OUModelOptimalThresholdZeng.sharpe_ratio

Plotting Comparison
*******************

One can use the following functions to observe the impact of transaction costs and risk-free rates on
the optimal thresholds and performance metrics under the optimal thresholds.

.. automethod:: OUModelOptimalThresholdZeng.plot_target_vs_c

.. automethod:: OUModelOptimalThresholdZeng.plot_sharpe_ratio_vs_rf

Examples
########

Code Example
************

.. code-block::

    # Importing packages
    import numpy as np
    import matplotlib.pyplot as plt
    from arbitragelab.time_series_approach.ou_optimal_threshold_zeng import OUModelOptimalThresholdZeng

    # Creating a class instance
    OUOTZ = OUModelOptimalThresholdZeng()

    # Initializing OU-process parameter
    OUOTZ.construct_ou_model_from_given_parameters(theta = 3.4241, mu = 0.0237, sigma = 0.0081)

    # Getting optimal thresholds by Conventional Optimal Rule.
    a_s, b_s, a_l, b_l = OUOTZ.get_threshold_by_conventional_optimal_rule(c = 0.02)

    print("Entering a short position when Xt =", a_s)
    print("Exiting a short position when Xt =", b_s)
    print("Entering a long position when Xt =", a_l)
    print("Exiting a long position when Xt =", b_l)

    # Getting the expected return and the variance for both long and short trade
    E_s = OUOTZ.expected_return(a = a_s, b = b_s, c = 0.02)
    V_s = OUOTZ.return_variance(a = a_s, b = b_s, c = 0.02)
    E_l = OUOTZ.expected_return(a = a_l, b = b_l, c = 0.02)
    V_l = OUOTZ.return_variance(a = a_l, b = b_l, c = 0.02)

    print("Short trade expected return:", E_s)
    print("Short trade variance:", V_s)
    print("Long trade expected return:", E_l)
    print("Long trade variance:", V_l)

    # Getting optimal thresholds by New Optimal Rule.
    a_s, b_s, a_l, b_l = OUOTZ.get_threshold_by_new_optimal_rule(c = 0.02)

    print("Entering a short position when Xt =", a_s)
    print("Exiting a short position when Xt =", b_s)
    print("Entering a long position when Xt =", a_l)
    print("Exiting a long position when Xt =", b_l)

    # Getting the expected return and the variance for both long and short trade
    E_s = OUOTZ.expected_return(a = a_s, b = b_s, c = 0.02)
    V_s = OUOTZ.return_variance(a = a_s, b = b_s, c = 0.02)
    E_l = OUOTZ.expected_return(a = a_l, b = b_l, c = 0.02)
    V_l = OUOTZ.return_variance(a = a_l, b = b_l, c = 0.02)

    print("Short trade expected return:", E_s)
    print("Short trade variance:", V_s)
    print("Long trade expected return:", E_l)
    print("Long trade variance:", V_l)

    # Setting a array contains transaction costs
    c_list = np.linspace(0, 0.01, 30)

    # Comparison of the expected return between the Conventional Optimal Rule and New Optimal Rule.
    c_list = np.linspace(0, 0.02, 30)
    fig_con = OUOTZ.plot_target_vs_c(target = "expected_return",
                                     method = "conventional_optimal_rule",
                                     c_list = c_list)
    fig_new = OUOTZ.plot_target_vs_c(target = "expected_return",
                                     method = "new_optimal_rule",
                                     c_list = c_list)
    plt.show()

    # Combining two figures.
    ax_con = fig_con.gca()
    ax_new = fig_new.gca()

    x = ax_con.lines[0].get_xdata()
    y_con = ax_con.lines[0].get_ydata()
    y_new = ax_new.lines[0].get_ydata()

    plt.plot(x, y_con, label = "Conventional Optimal Rule")
    plt.plot(x, y_new, label = "New Optimal Rule")
    plt.legend()
    plt.show()

Research Notebooks
******************

The following research notebook can be used to better understand the method described above.

* `OU Model Optimal Trading Thresholds Zeng`_

.. _`OU Model Optimal Trading Thresholds Zeng`: https://hudsonthames.org/notebooks/arblab/ou_model_optimal_threshold_Zeng.html

.. raw:: html

    <a href="https://hudthames.tech/3q11ATN"><button style="margin: 20px; margin-top: 0px">Download Notebook</button></a>
    <a href="https://hudthames.tech/2S03R58"><button style="margin: 20px; margin-top: 0px">Download Sample Data</button></a>

References
##########

* `Zeng, Z. and Lee, C.-G., Pairs trading: optimal thresholds and profitability. Quantitative Finance, 14(11): 1881–1893 <https://www.tandfonline.com/doi/pdf/10.1080/14697688.2014.917806>`_.