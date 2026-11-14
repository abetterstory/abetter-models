# abetter-models

Model files used by the A Better Story starter, published here so image builds can fetch them without a token.

## embed-siglip-b16-224-int8-1

`siglip-b16-224-vision-int8.onnx`: the vision tower of SigLIP base, patch 16, 224 px
([google/siglip-base-patch16-224](https://huggingface.co/google/siglip-base-patch16-224)), exported to ONNX and quantised.

- **How it was made:** `SiglipVisionModel` exported with `torch.onnx.export` at opset 17 (input `pixel_values`
  [batch, 3, 224, 224]; outputs `last_hidden_state`, `pooler_output` [batch, 768]), then
  `onnxruntime.quantization.quantize_dynamic(..., weight_type=QuantType.QInt8, op_types_to_quantize=["MatMul"])`:
  int8 MatMul weights, the patch-embedding Conv kept in float.
- **Checked against the float model** on nine images (seven photos, two synthetic), cosine of `pooler_output`:
  min 0.971, mean 0.985, max 0.995.
- **sha256:** `f7ac4fc8f8f5205918c16a58fd67e46c45e286132e7931f8145cd52fd7c54ea6` (101,246,777 bytes).
- The starter pins this file by its sha256 and refuses any other bytes. A changed file is a new release tag, never a
  replaced asset.
- No data of ours went into it: it is Google's published weights, exported and quantised.

## Licence

The model weights are Google's, under the Apache License 2.0 (`LICENSE`). The files here are a modified form of them
(the export and the quantisation are the changes); see `NOTICE`.
