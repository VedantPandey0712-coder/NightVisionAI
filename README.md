# NightVision AI – Adaptive Low-Light Enhancement & Detection

Major project (B.Tech CSE, PSIT Kanpur). See the synopsis for the full plan.
This repository currently contains **Milestone 1: the night-to-visible image enhancer
and the enhancement half of the Adaptive Night Detection Pipeline (ANDP)**.

## How it works
1. **Analyse** – mean luminance, RMS contrast, noise sigma (`nightvision/lighting.py`).
2. **Classify** – bright / normal / dim / very_dark (thresholds tunable on ExDark val).
3. **Choose a path** – well-lit frames are left untouched; dim frames get partial
   Zero-DCE; very dark frames get pre-denoise → full Zero-DCE → brightness lift → denoise.
4. Methods: `zero_dce` (official pretrained weights, bundled), `retinex`, `gamma_clahe`.

## Use
```bash
pip install -r requirements.txt
python -m nightvision.cli night.jpg -o out.jpg --compare      # original | enhanced
python tests/make_samples.py samples && python tests/benchmark.py
pytest -q
```
```python
import cv2; from nightvision import enhance_image
cv2.imwrite("out.jpg", enhance_image(cv2.imread("night.jpg")))
```

## Roadmap (from synopsis)
- [x] M1 Enhancer + lighting analysis + CLI + tests
- [ ] M2 ExDark prep, YOLO26 baseline, enhance-vs-no-enhance mAP, validation gate, per-class thresholds
- [ ] M3 Django REST + React side-by-side viewer, history, analytics
- [ ] M4 Video (ByteTrack, Celery/Redis), PDF reports, Docker
Zero-DCE weights: official repo (Li-Chongyi/Zero-DCE), check its licence before redistribution.
