.PHONY: install setup train clean

# Variables
PYTHON := python3
PDM := pdm
TRAIN_SCRIPT := train.py

# Default target
all: setup install train

# Install PDM
setup:
	@echo "Installing PDM..."
	@curl -sSL https://pdm.fming.dev/install-pdm.py | $(PYTHON) -
	@echo "PDM installation completed"

# Install dependencies using PDM
install:
	@echo "Installing dependencies..."
	@$(PDM) install
	@echo "Dependencies installation completed"

# Train the model with default parameters
train:
	@echo "Training model..."
	@$(PDM) run python $(TRAIN_SCRIPT) \
        --max-iter 200 \
        --solver lbfgs \
        --random-state 42 \
        --test-size 0.2
	@echo "Training completed"

# Clean up generated files
clean:
	@echo "Cleaning up..."
	@rm -f iris_model.pkl
	@rm -rf __pycache__
	@rm -rf .pdm-build
	@rm -rf dist
	@echo "Cleanup completed"