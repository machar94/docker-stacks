FROM illumidesk/base-notebook:latest AS base

FROM illumidesk/r-conda:latest

ENV NB_USER=jovyan
ENV NB_UID=1000
ENV NB_GID=100
ENV HOME="/home/${NB_USER}"

USER root

RUN mkdir -p /etc/jupyter

# copy files from base image
COPY --from=base /usr/local/bin/start* /usr/local/bin/
COPY --from=base /usr/local/bin/fix-permissions /usr/local/bin/
COPY --from=base /etc/jupyter/jupyter_notebook_config.py /etc/jupyter/

# update permissions
RUN chmod a+rx /usr/local/bin/fix-permissions
RUN chmod +rx /usr/local/bin/start*
RUN chmod +rx /etc/jupyter/jupyter_notebook_config.py

# copy rstudio configuration
COPY rserver.conf /etc/rstudio/rserver.conf

# copy jupyter notebook config
COPY jupyter_notebook_config.py /etc/jupyter/jupyter_notebook_config.py

# Fix permissions on /etc/jupyter and /etc/rstudio as root
RUN fix-permissions /etc/jupyter/ \
 && fix-permissions /etc/rstudio/ \
 && fix-permissions "${HOME}"

# Build lab extensions that may have been included
RUN jupyter lab build

USER "${NB_UID}"

WORKDIR "${HOME}"
