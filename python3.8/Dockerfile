FROM scratch as runtime
# Copy extracted python runtime
ADD ./python3.8-extracted-runtime.tar.gz /opt

FROM baseimage

# Copy the runtime into base image
COPY --from=runtime /opt/* /var/

ENV PATH=/var/lang/bin:$PATH \
    LD_LIBRARY_PATH=/var/lang/lib:$LD_LIBRARY_PATH \
    AWS_EXECUTION_ENV=AWS_Lambda_python3.8 \
    PKG_CONFIG_PATH=/var/lang/lib/pkgconfig:/usr/lib64/pkgconfig:/usr/share/pkgconfig \
    PIPX_BIN_DIR=/var/lang/bin \
    PIPX_HOME=/var/lang/pipx

# Update and install python dependencies
RUN pip install -U pip setuptools wheel --no-cache-dir && \
    pip install pipx --no-cache-dir && \
    pipx install virtualenv && \
    pipx install pipenv && \
    pipx install poetry==1.1.4 && \
    pipx install awscli==1.* && \
    pipx install aws-lambda-builders==1.2.0 && \
    pipx install aws-sam-cli==1.15.0


USER sbx_user1051

ENTRYPOINT ["/var/rapid/init", "--bootstrap", "/var/runtime/bootstrap", "--enable-msg-logs"]

