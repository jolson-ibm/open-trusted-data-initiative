PYTHON?=python
DOCKER?=docker

.PHONY: install
.PHONY: samples run_python_samples run_ray_samples
.PHONY: tests run_python_tests run_ray_tests
.PHONY: images build_python_image build_ray_image

samples:: run_python_samples run_ray_samples
tests::   run_python_tests run_ray_tests
images::  build_python_image build_ray_image

install::
	pip install --no-cache-dir -e .

run_python_samples:: install
	${PYTHON} src/hf_dataset_explorer/python/dataset_explorer_local.py
	${PYTHON} src/hf_dataset_explorer/python/dataset_explorer_local_python.py

run_python_tests::
	pip install --no-cache-dir -e .
	pytest test/python

build_python_image::
	${DOCKER} build . -t hf_dataset_explorer_python -f Dockerfile_python

run_ray_samples::
	pip install --no-cache-dir -e .
	${PYTHON} src/hf_dataset_explorer/ray/dataset_explorer_local_ray.py

run_ray_tests::
	pip install --no-cache-dir -e .
	pytest test/ray


build_ray_image::
	${DOCKER} build . -t hf_dataset_explorer_python -f Dockerfile_ray