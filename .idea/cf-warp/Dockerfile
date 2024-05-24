FROM ubuntu:22.10
RUN apt update && apt install -y curl gpg supervisor && curl https://pkg.cloudflareclient.com/pubkey.gpg | gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg && echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ jammy main" | tee /etc/apt/sources.list.d/cloudflare-client.list && apt update && apt install cloudflare-warp -y && apt clean
RUN echo "[program:warp-svc]\ncommand=/bin/bash -c /usr/bin/warp-svc\nautostart=true\nstartretries=3\nstderr_logfile=/var/log/warp.log\nstdout_logfile=/var/log/warp.log\n" > /etc/supervisor/conf.d/warp.conf
RUN echo "supervisor\nsleep 5\nwarp-cli --accept-tos register\nwarp-cli --accept-tos set-mode proxy\nwarp-cli --accept-tos connect\ntail -f /var/log/warp.log\n" > /init.sh && chmod +x /init.sh
EXPOSE "40000/tcp"
CMD ["bash", "-c", "/init.sh"]

# docker build -t warp:latest -f Dockerfile ./
