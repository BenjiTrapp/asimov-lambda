######
## Stage 1: Build an Amazonlinux flavoured Image and inject the captured files
######
FROM amazonlinux:2 as builder

# Copy manual extracted files which were captured from AWS Lambda
ADD ./extracted_files.tar.gz /asimovlambda/

# Install the required AWS Dependencies
RUN yum --installroot=/asimovlambda reinstall -y  \
                                    filesystem-3.2-25.amzn2.0.4 \
                                    setup-2.8.71-10.amzn2.0.1 \
                                    glibc-2.26-39.amzn2 \
                                    glibc-common-2.26-39.amzn2 \
  # Remove the garbage
  && yum --installroot=/asimovlambda clean all

######
## Stage 2: Create a smaller Asimov Lambda BaseImage
######
FROM scratch

# Copy the installed OS files to the root dir
COPY --from=builder /asimovlambda /

ENV PATH=/usr/local/bin:/usr/bin/:/bin:/opt/bin \
    LD_LIBRARY_PATH=/lib64:/usr/lib64:/var/runtime:/var/runtime/lib:/var/task:/var/task/lib:/opt/lib \
    LANG=en_US.UTF-8 \
    TZ=:UTC \
    LAMBDA_TASK_ROOT=/var/task \
    LAMBDA_RUNTIME_DIR=/var/runtime \
    _LAMBDA_CONTROL_SOCKET=14 \
    _LAMBDA_SHARED_MEM_FD=11 \
    _LAMBDA_LOG_FD=9 \
    _LAMBDA_SB_ID=7 \
    _LAMBDA_CONSOLE_SOCKET=16 \
    _LAMBDA_RUNTIME_LOAD_TIME=1530232235231 \
    _AWS_XRAY_DAEMON_ADDRESS=169.254.79.129 \
    _AWS_XRAY_DAEMON_PORT=2000 \
    _X_AMZN_TRACE_ID='Root=1-6217f7ab-1f6b3ebf2a1811227263a659;Parent=5c39ff32e875095b;Sampled=1' \
    AWS_XRAY_DAEMON_ADDRESS=169.254.79.2:2000 \
    AWS_XRAY_CONTEXT_MISSING=LOG_ERROR 

# fix permissions
RUN chown sbx_user1051:495 /tmp && \
    chmod 700 /tmp

# Switch to the task dir to get ready to inject the Code
WORKDIR /var/task
