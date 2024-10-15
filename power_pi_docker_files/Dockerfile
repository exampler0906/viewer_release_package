# 选择基础镜像
FROM sd-ubuntu-backend:test1

# 设置镜像的作者信息（可选）
LABEL maintainer="exampler0906"

# 安装python环境
RUN apt update
RUN apt upgrade -y
RUN apt install -y python3 python3-pip
RUN /bin/pip3 install numpy  vtk commentjson pyevtk pandas openpyxl xlsxwriter  -i https://pypi.tuna.tsinghua.edu.cn/simple

# 安装power-pi
RUN mkdir -p /root/power_pi
COPY debug.tar /root/power_pi
RUN tar -xvf /root/power_pi/debug.tar
COPY release.tar /root/power_pi
RUN tar -xvf /root/power_pi/release.tar
ENV LD_LIBRARY_PATH="/root/debug/lib:/root/Release:${LD_LIBRARY_PATH}"

# 安装conda
RUN mkdir -p /root/conda
COPY ./conda/Anaconda3-2024.02-1-Linux-x86_64.sh /root/conda
COPY ./conda/unisolver1.yaml /root/conda
RUN chmod +x /root/conda/Anaconda3-2024.02-1-Linux-x86_64.sh
RUN /root/conda/Anaconda3-2024.02-1-Linux-x86_64.sh -b
ENV PATH="${PATH}:/root/anaconda3/bin"
RUN conda env create -f  /root/conda/unisolver1.yaml
RUN conda init
RUN /bin/bash

# 配置power-pi桌面快捷方式
COPY power_pi_start.sh /root/power_pi/
RUN chmod +x /root/power_pi/power_pi_start.sh
RUN mkdir -p /root/Desktop
COPY power_pi_start.desktop /root/Desktop/
RUN chmod +x /root/Desktop/power_pi_start.desktop

# 便于日后更新
COPY Power-PI /root/Release/
RUN cp -r /root/Release/project_demo /root/
RUN cp -r /root/Release/res /root/
ENV LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:/root/anaconda3/envs/unisolver/lib"

# 设置默认启动命令为执行脚本
CMD ["/bin/sh"]