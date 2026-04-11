
# Model Architectures and Training Details

## Baseline 1: GCN
- Feature Extractor: ResNet18 (pretrained on ImageNet)
- Input Features: 512-dimensional
- Graph Construction: 8-nearest neighbors in feature space
- GCN Architecture: 
  * Layer 1: Input (512) -> Hidden (128)
  * Layer 2: Hidden (128) -> Output (2 classes)
- Training: 20 epochs, Adam optimizer (lr=0.01), Cross-entropy loss
- Dataset: PatchCamelyon (5,000 patches, 128×128)

## Baseline 2: PRGAT
- Feature Extractor: ResNet18 (same as GCN)
- Input Features: 513-dimensional (512-d features + PageRank score)
- Graph Construction: 8-NN in feature space
- PageRank Integration: Multiplicative reweighting in attention mechanism
- Architecture:
  * GAT Layer 1: Input (513) -> Hidden (128×4=512, 4 attention heads)
  * GAT Layer 2: Hidden (512) -> Output (2 classes, 1 head)
- Training: 20 epochs, Adam optimizer (lr=0.005, weight_decay=5e-4)
- Dataset: PatchCamelyon (5,000 patches)

## Proposed: PageRankGNN (WSI Pipeline)
- Feature Extractor: ViT-Base patch16/224 (pretrained on ImageNet-21K)
- Input Features: 768-dimensional
- Graph Construction: 6-nearest neighbors in spatial coordinate space
- PageRank Integration: Spatial weighting of model embeddings
- Architecture:
  * GAT Layer: Input (768) -> 128 features, 4 attention heads
  * Fully Connected: (512) -> Output probability [0, 1] (sigmoid)
  * PageRank multiplicative weighting: x_new = x + 0.1 * lr_score * x
- Training: 50 epochs, Adam optimizer (lr=1e-4), BCEWithLogitsLoss
- Dataset: PANDA WSI (multiple slides with Gleason grades, ~300 patches/slide)
- Evaluation: Slide-level aggregation via patch mean
