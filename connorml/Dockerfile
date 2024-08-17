FROM pytorch/pytorch:latest

RUN apt update && apt install -y git openssh-server wget tmux vim ripgrep htop nvtop

# Install Neovim from source
RUN apt install -y ninja-build gettext libtool libtool-bin autoconf automake cmake g++ pkg-config unzip npm

RUN git clone https://github.com/neovim/neovim.git ./neovim
WORKDIR ./neovim
RUN pwd && ls -la && make CMAKE_BUILD_TYPE=Release
RUN make install

# Clone Neovim repository
RUN git clone https://github.com/conacts/neovim ~/.config/nvim

# Clone packer.nvim
RUN git clone --depth 1 https://github.com/wbthomason/packer.nvim ~/.local/share/nvim/site/pack/packer/start/packer.nvim

# Clone Tmux Plugin Manager (tpm)
RUN git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm

# Run PackerSync in Neovim
# RUN nvim --headless +PackerSync +qall

# Configure Git
RUN git config --global credential.helper store

# Pull the dotfiles repository
RUN git clone https://github.com/conacts/dotfiles.git ./dotfiles

RUN cp ./dotfiles/config/.tmux.conf ~/.tmux.conf
RUN cp ./dotfiles/config/.bashrc ~/.bashrc
RUN cp ./dotfiles/config/.vimrc ~/.vimrc

RUN pip3 install datasets transformers accelerate torchvision lightning scikit-learn peft
RUN pip3 install jupyterlab jupyterlab-lsp jupyterlab-git jupyterlab-system-monitor
RUN pip3 install wandb torchmetrics matplotlib seaborn ipython
RUN pip3 install bitsandbytes ruff langchain gradio openai tiktoken fire # llama_recipe

# Expose ports for communication
EXPOSE 8888
EXPOSE 3000

# Set environment variables
ENV PYTHONUNBUFFERED=1

RUN echo "Login to HF & W&B"
