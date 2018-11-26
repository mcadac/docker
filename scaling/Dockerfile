FROM nginx:latest

RUN echo UP > /usr/share/nginx/html/status

RUN echo "[[ \"\$(cat /usr/share/nginx/html/status)\" == \"UP\" ]] || (echo UP > /usr/share/nginx/html/status && nginx -s stop) " > /bin/healthcheck \
	&& chmod +x /bin/healthcheck \
	&& /bin/bash /bin/healthcheck

HEALTHCHECK --interval=2s --retries=1 --start-period=30s CMD /bin/bash healthcheck

CMD hostname > /usr/share/nginx/html/hostname && nginx -g "daemon off;"

