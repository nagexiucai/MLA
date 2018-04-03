# 安装配置

## 典型安装
- pip install jupyter

## 新内核配置
- Python
  - NEW-VERSION-PYTHON -m pip install ipykernel
  - NEW-VERSION-PYTHON -m ipykernel install

## 远程访问配置
- 命令行参数
  - jupyter notebook --no-browser --port=PORT --ip=IP --alow-root
  - http://IP:PORT/tree?token=TOKEN