# Week 4 Model Analysis Memo

**Product Anchor:** Aido Rover and Sentinel Prime AI  
**Analysis Focus:** Model Performance, Robustness, and Cross-Platform Transfer

## 1. Model Selection Rationale

Random Forest was selected as the preferred classifier for Aido Rover operational mode prediction because it consistently outperformed Logistic Regression. In five-fold stratified cross-validation, Random Forest achieved a mean Macro-F1 of **0.9307 ± 0.0008**, compared with **0.7376 ± 0.0023** for Logistic Regression. The small standard deviation across Random Forest folds indicates that its performance was highly stable across different training subsets rather than being dependent on a favorable data split.

The performance difference also suggests that the relationships between Aido Rover sensor telemetry and operational modes are not fully captured by a linear decision boundary. Random Forest can model nonlinear relationships and interactions among multiple sensor variables, making it better suited to the telemetry classification task. Based on both predictive performance and cross-validation stability, Random Forest was retained as the primary model for subsequent robustness analysis.

## 2. Key Feature Drivers

Permutation feature importance identified `wheel_imbalance`, `wheel_torque_fr`, and `wheel_torque_fl` as the three strongest predictors of Aido Rover operational mode. `wheel_imbalance` was the most influential feature, with its permutation reducing Macro-F1 by approximately 0.30. The two front-wheel torque measurements followed, each producing a decrease of roughly 0.20. Impurity-based importance also ranked the individual wheel torque measurements highly, providing additional evidence that wheel-related telemetry is central to the model's predictions.

The importance of `wheel_imbalance` was especially relevant to FAULT detection. In a separate FAULT-versus-PATROL permutation analysis, it again produced the largest performance decrease, followed by the front-wheel torque measurements. This suggests that differences in torque across the Rover's wheels provide useful information for distinguishing routine patrol behavior from abnormal operating conditions.

Operationally, wheel imbalance and individual torque measurements could help identify observations that warrant closer inspection, since unusual torque patterns may reflect uneven loading, traction differences, drivetrain issues, or other mechanical abnormalities. However, because the telemetry is synthetic, these relationships reflect the simulated data-generating assumptions and should be validated using real Aido Rover telemetry before being treated as actual hardware behavior.

## 3. Class Imbalance Finding

The Aido Rover dataset showed substantial class imbalance. PATROL represented approximately **70.0%** of the training observations, while FAULT accounted for only **4.9%**. Although the baseline Random Forest achieved strong overall performance, FAULT remained the most difficult operational mode to detect. Its recall was **0.7858**, meaning that approximately 21.4% of actual FAULT observations in the test set were missed. Of the 384 missed FAULT observations, 339 were incorrectly classified as PATROL.

Class weighting was evaluated to determine whether giving greater importance to minority classes would improve FAULT detection. However, the class-weighted Random Forest reduced FAULT recall from **0.7858 to 0.7680** and slightly decreased Macro-F1 from **0.9298 to 0.9277**. Because class weighting did not improve minority-class detection, the original Random Forest was retained.

From an operational perspective, missed FAULT events are particularly important because a Rover experiencing abnormal behavior could continue to be interpreted as operating normally. The concentration of FAULT errors in the PATROL class therefore represents a more significant monitoring concern than the overall accuracy alone would suggest. These results suggest that class imbalance alone may not fully explain the remaining FAULT classification errors. Future work could investigate additional fault-sensitive features or alternative imbalance-handling methods to improve detection without substantially increasing false alarms.

## 4. Per-platform Deployment Recommendation

The transfer test showed that strong performance on Aido Rover does not automatically generalize to another InGen platform. When the Aido-trained Random Forest was applied directly to the synthetic Sentinel Prime AI dataset without retraining, accuracy decreased from **0.9662 to 0.7084**, while Macro-F1 decreased from **0.9298 to 0.6387**.

The performance decline also varied substantially across operational modes. CHARGING remained highly transferable, with a recall of **0.9932**, while ALERT recall fell to only **0.1882**. FAULT recall remained high at **0.8914**, but its precision decreased to **0.3587**, indicating that many Sentinel observations were incorrectly flagged as FAULT. These differences suggest that operational patterns learned from Aido telemetry do not transfer consistently when the underlying sensor distributions change.

Based on this experiment, a single Aido-trained classifier should not be directly deployed across multiple InGen platforms without validation. Platform-specific training or model adaptation is recommended when sufficient platform data are available. A shared modeling framework and feature-engineering pipeline may still be reusable across platforms, but each model should be evaluated and calibrated using telemetry representative of its intended deployment environment.

The Sentinel dataset used in this transfer test was synthetic, and the cross-platform telemetry shifts were simulated rather than derived from production Sentinel data. Therefore, the observed **0.7084 transfer accuracy** should be interpreted as evidence from a controlled robustness experiment, not as an estimate of actual Sentinel Prime AI production performance.