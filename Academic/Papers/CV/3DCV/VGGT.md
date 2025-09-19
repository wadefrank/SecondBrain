
# 简介

homepage: https://vgg-t.github.io/

github: https://github.com/facebookresearch/vggt

arxiv: https://arxiv.org/abs/2503.11651

pdf: https://arxiv.org/pdf/2503.11651

# 输入输出

- input: a sequence of N RGB images (N = 1, …)
- output (for each image):
    - camera parameters: g_i ∈ R^9 (intrinsics and extrinsics)
    - depth map: D_i ∈ R^(H x W)
    - point map: P_i ∈ R^(3 x H x W)
    - a grid T_i ∈ R(C × H × W) of C-dimensional features for point tracking