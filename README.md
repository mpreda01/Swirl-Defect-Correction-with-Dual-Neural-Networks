# Swirl-Defect-Correction-with-Dual-Neural-Networks
Dual-network pipeline for swirl defect removal. A U-Net predicts defect masks, which guide a second network to selectively correct only damaged regions. Optimized under a 4M parameter limit with custom loss and strong generalization performance.


# EVALUATION LOGIC 
The evaluation routine is designed to quantify the effectiveness of the correction network.
It computes the ratio between the residual error (after correction) and the original error
(before correction), but crucially, it focuses the computation only on the defect region
using the ground truth mask (not the predicted one).

Evaluation Steps:

For each batch in the test set:

Load the swirled (defected) image, its ground truth mask, and the original clean image.

Compute the initial MSE between the swirled and clean images within the mask region.
This represents the amount of damage introduced by the swirl defect.

Predict the mask using the first network.

Use the predicted mask as additional input to the second network
to obtain the corrected image.

Compute the corrected MSE between the corrected image and the clean image,
again only within the ground truth defect region.

For each image in the batch:

Compute the ratio: corrected_MSE / initial_MSE_batch_mean
This shows how much the correction improved the image relative to the original defect.

Final Metrics:

The mean and standard deviation of these per-image ratios across the test set
provide a concise and interpretable measure of performance.

Mean ratio < 0.5 → strong correction

Mean ratio ≈ 1.0 → ineffective correction

Std. deviation → robustness & consistency of the correction

Note:
The evaluation uses the batch mean for initial error in the ratio denominator
instead of per-image error. This stabilizes the ratio metric and reduces
the sensitivity to images with minimal damage.

