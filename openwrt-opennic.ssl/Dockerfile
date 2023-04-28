FROM scratch

ENV HETZNER_Token="Hetzner-API-Token"
ENV DNSRECORD="foo.bar.tld"
ENV DNSEMAIL="foo@bar.tld"

#ADD https://downloads.openwrt.org/releases/22.03.3/targets/x86/64/openwrt-22.03.3-x86-64-rootfs.tar.gz /
#ADD https://downloads.openwrt.org/releases/22.03.4/targets/armvirt/64/openwrt-22.03.4-armvirt-64-default-rootfs.tar.gz /
ADD openwrt-22.03.4-armvirt-64-default-rootfs.tar.gz /

COPY srvzone /
COPY srvzone.conf /

RUN	mkdir -p /var/lock/ && opkg update && \
 	opkg install wget bash bind-server bind-tools bind-rndc bind-dnssec bind-dig && \
	opkg install acme acme-dnsapi && \
	opkg install libopenssl wget-ssl && \
	opkg remove dnsmasq && \
	/etc/init.d/dropbear disable && \
	/etc/init.d/uhttpd disable && \
	/etc/init.d/firewall disable && \
	openssl dhparam -out /etc/bind/dhparam.pem 1024 && \
	mkdir -p /etc/crontabs && \
	touch /etc/crontabs/root && \
	cd /usr/lib/acme/ && \
	./acme.sh --install -m $DNSEMAIL && \ 
	cd / && \
	mkdir -p /root/.acme.sh && \
	/root/.acme.sh/acme.sh --log --upgrade && \
	/root/.acme.sh/acme.sh --set-default-ca --server letsencrypt && \
	/root/.acme.sh/acme.sh --log --issue --dns dns_hetzner -d $DNSRECORD --dnssleep 30 && \
	ln -s /usr/bin/named-checkconf /usr/sbin && \
	ln -s /root/.acme.sh/$(echo $DNSRECORD)_ecc/$(echo $DNSRECORD).key /etc/bind/privkey.pem && \
	ln -s /root/.acme.sh/$(echo $DNSRECORD)_ecc/fullchain.cer /etc/bind/fullchain.pem && \
	chmod 755 /root/.acme.sh && \
	chmod 600 /root/.acme.sh/account.conf && \
	chmod 644 /root/.acme.sh/$(echo $DNSRECORD)_ecc/$(echo $DNSRECORD).key && \
	bash /srvzone && \
	mkdir -p /tmp/bind/opennic/slave && \
	chown -R bind:bind /tmp/bind/opennic && \
	ln -s /tmp/bind/opennic /etc/bind

COPY resolv.conf /etc/
COPY named.conf /etc/bind/
COPY named.conf.options /etc/bind/
COPY crontab /etc/crontabs/root
COPY firewall /etc/config
COPY rc.local /etc/

EXPOSE 53/udp 53/tcp 443/tcp 853/tcp 

USER root

CMD ["/sbin/init"]

