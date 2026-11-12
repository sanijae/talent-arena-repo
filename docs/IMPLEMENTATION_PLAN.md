MORE THAN A HACK 2027
Winning Algorithm — Technical Implementation Plan

Project: Football Team Possession Estimation
Primary Objective: Maximize spellF1 and achieve the highest possible competition ranking.
Approach: Rule-based possession estimation with multi-signal analysis and temporal reasoning.

1. PROJECT OBJECTIVE

Develop a robust algorithm that predicts the possession state of every frame in the provided football match.

The algorithm must output one of four labels:

H — Home team possession.
A — Away team possession.
D — Ball dead / out of play.
X — Abstain / no answer.

We will initially use the supplied tracking data, Smart Tagging and other permitted generated data.

We will NOT train, fit or calibrate the algorithm using the competition's ground-truth possession labels or provided video.

2. CORE STRATEGY

Build a multi-stage rule-based algorithm rather than relying on the nearest-player-to-ball method alone.

The algorithm must understand:

Which team is most likely controlling the ball.
Whether the ball is moving between teammates.
Whether an opponent has intercepted or recovered the ball.
Whether the ball is contested or loose.
Whether play has stopped.
Whether temporary missing tracking data should preserve the previous possession state.

Prioritize possession continuity and accurate possession transitions.

3. SYSTEM ARCHITECTURE
Module 1: Data Loading and Preprocessing
Load all provided tracking data.
Load Smart Tagging and relevant metadata.
Align records using the correct frame identifiers and timestamps.
Identify home and away players.
Separate ball observations from player observations.
Detect missing values, duplicate frames and tracking inconsistencies.
Preserve the original data and create clean working datasets.

Output: Clean, frame-aligned tracking data.

Module 2: Tracking Quality Assessment

For every frame, calculate:

Ball visibility and tracking confidence, where available.
Player tracking availability.
Missing observations.
Unusual coordinate jumps.
Potential identity switches.
Tracking reliability.

Do not automatically interpret a missing ball observation as a dead ball.

Output: Tracking reliability information for each frame.

Module 3: Ball-Player Interaction Analysis

For each frame, calculate:

Distance between the ball and every player.
Closest home player.
Closest away player.
Number of nearby teammates and opponents.
Ball movement between consecutive frames.
Player movement and direction.
Whether the ball is approaching, leaving or moving with a player.

Use pitch coordinates where reliable.

Do not use distance alone to establish possession.

Output: Candidate possession team and interaction features.

Module 4: Team Control Estimation

Develop rules that combine:

Ball-player distance.
Player movement.
Ball movement.
Team identity.
Evidence of controlled touches.
Evidence of interceptions.
Tracking reliability.

Estimate whether Home or Away has control.

Handle contested situations separately instead of automatically assigning possession to the nearest player.

Output: Home, Away or uncertain control evidence.

Module 5: Dead-Ball Detection

Build a dedicated module for identifying dead-ball situations.

Use available information such as:

Smart Tagging events.
Ball position and movement.
Pitch boundaries.
Restart situations.
Match stoppage information, where available.

Distinguish actual dead-ball periods from temporary tracking failures.

Output: Dead-ball evidence for every frame.

Module 6: Temporal Possession Reasoning

This is one of the most important modules.

Possession should be treated as a sequence, not as independent frame predictions.

Implement:

Possession continuity rules.
Transition detection.
Temporary tracking-loss handling.
Controlled possession changes.
Loose-ball and contested-ball handling.
Short-term state memory.

Investigate a Hidden Markov Model or semi-Markov approach as an optional enhancement, using only externally trained or otherwise permitted parameters.

Avoid arbitrary fixed smoothing windows as the final solution.

Output: Temporally consistent possession predictions.

Module 7: Final Decision Engine

Combine the outputs from all previous modules.

Apply decision priorities:

Confirmed dead-ball evidence.
Strong evidence of team control.
Strong evidence of possession transition.
Previous possession continuity when observations are temporarily unreliable.
Abstention when evidence is genuinely insufficient.

The final output must contain only valid competition labels: H, A, D or X.

Do not let the previous possession state override convincing evidence of a genuine possession change.

4. RESEARCH AND IMPROVEMENT STRATEGY

After implementing the first working algorithm, investigate these improvements:

A. Adaptive temporal reasoning

Adjust how strongly the algorithm preserves previous possession according to tracking reliability and evidence of a new possession.

B. Possession transition detection

Develop dedicated rules for interceptions, tackles resulting in control, rebounds and loose-ball recoveries.

C. Missing ball recovery

Use surrounding player movement and previous possession information to handle temporary ball-tracking gaps.

D. Multi-source information fusion

Investigate whether Smart Tagging and other generated data improve predictions beyond tracking data alone.

E. External machine learning

Optionally investigate models trained on legally usable external football datasets. Never use the competition ground truth or provided video for training, fitting or calibration.

Only retain improvements that demonstrate value on permitted independent external validation.

5. EVALUATION STRATEGY

The primary target is spellF1.

The secondary metric is macroF1.

Our evaluation process must:

Reproduce the official baseline.
Understand the official scoring implementation.
Use external validation data for tuning and model selection.
Compare every new component against the existing baseline.
Run ablation experiments to measure each component's contribution.
Monitor false possession transitions.
Monitor missed possession transitions.
Monitor dead-ball classification.
Check performance across different match situations.

Do not compare predictions with the competition's ground-truth labels or use the provided video to correct or tune the algorithm.

Do not use the public ground-truth sample for training or tuning.

6. EXPERIMENT MANAGEMENT

Maintain separate versions of the algorithm:

Version 1: Reproduced official baseline.
Version 2: Improved ball-player interaction.
Version 3: Dead-ball detection.
Version 4: Temporal possession reasoning.
Version 5: Integrated multi-signal algorithm.
Version 6: Final optimized submission.

Record every change, its rationale and its external validation results.

Never introduce a complicated component without evidence that it improves the system.

7. FINAL SUBMISSION REQUIREMENTS

Generate the exact CSV format required by the official repository.

Verify:

All required frames are included.
Frame identifiers are correct.
Every prediction is a valid label.
No duplicate or missing frame records exist.
The output format matches the submission instructions.
The algorithm reproduces the submitted CSV exactly.

Publish a reproducible public GitHub repository containing:

Source code.
Requirements file.
README.
Algorithm documentation.
Reproduction instructions.
Submission generation script.

Do not include competition data or restricted material in the public repository.

8. IMPLEMENTATION PRIORITIES

Follow this order:

Understand and reproduce the official baseline.
Build the data preprocessing pipeline.
Implement improved ball-player interaction rules.
Implement dedicated dead-ball detection.
Implement temporal possession reasoning.
Integrate Smart Tagging and other useful signals.
Evaluate improvements using permitted external validation.
Finalize and reproduce the complete submission.
FINAL DEVELOPMENT PRINCIPLE

Build the best possible possession-estimation algorithm, not the most complicated model.

Our competitive advantage should come from better football reasoning, reliable tracking interpretation, accurate possession transitions, intelligent handling of missing observations and temporal consistency.

Primary success criterion: Maximize spellF1 without violating any competition rule.