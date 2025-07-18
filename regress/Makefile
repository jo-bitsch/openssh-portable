#	$OpenBSD: Makefile,v 1.140 2025/07/04 07:52:17 djm Exp $

tests:		prep file-tests t-exec unit

REGRESS_TARGETS=	t1 t2 t3 t4 t5 t7 t9 t10 t11 t12

# File based tests
file-tests: $(REGRESS_TARGETS)

# Interop tests are not run by default
interop interop-tests: t-exec-interop

extra extra-tests: t-extra

prep:
	test "x${USE_VALGRIND}" = "x" || mkdir -p $(OBJ)/valgrind-out

clean:
	for F in $(CLEANFILES); do rm -f $(OBJ)$$F; done
	rm -rf $(OBJ).putty
	rm -rf $(OBJ).dropbear

distclean:	clean

LTESTS= 	connect \
		proxy-connect \
		sshfp-connect \
		connect-privsep \
		connect-uri \
		proto-version \
		proto-mismatch \
		exit-status \
		exit-status-signal \
		envpass \
		transfer \
		banner \
		rekey \
		dhgex \
		stderr-data \
		stderr-after-eof \
		broken-pipe \
		try-ciphers \
		yes-head \
		login-timeout \
		agent \
		agent-getpeereid \
		agent-timeout \
		agent-ptrace \
		agent-subprocess \
		keyscan \
		keygen-change \
		keygen-comment \
		keygen-convert \
		keygen-knownhosts \
		keygen-moduli \
		keygen-sshfp \
		key-options \
		scp \
		scp3 \
		scp-uri \
		sftp \
		sftp-chroot \
		sftp-cmds \
		sftp-badcmds \
		sftp-batch \
		sftp-glob \
		sftp-perm \
		sftp-resume \
		sftp-uri \
		reconfigure \
		dynamic-forward \
		forwarding \
		multiplex \
		reexec \
		brokenkeys \
		sshcfgparse \
		cfgparse \
		cfgmatch \
		cfgmatchlisten \
		percent \
		addrmatch \
		localcommand \
		forcecommand \
		portnum \
		keytype \
		kextype \
		cert-hostkey \
		cert-userkey \
		host-expand \
		keys-command \
		forward-control \
		integrity \
		krl \
		multipubkey \
		limit-keytype \
		hostkey-agent \
		hostkey-rotate \
		principals-command \
		cert-file \
		cfginclude \
		servcfginclude \
		allow-deny-users \
		authinfo \
		sshsig \
		knownhosts \
		knownhosts-command \
		agent-restrict \
		hostbased \
		password \
		channel-timeout \
		connection-timeout \
		match-subsystem \
		agent-pkcs11-restrict \
		agent-pkcs11-cert \
		penalty \
		penalty-expire \
		connect-bigconf \
		webauthn-in-agent 

INTEROP_TESTS=	putty-transfer putty-ciphers putty-kex conch-ciphers
INTEROP_TESTS+=	dropbear-ciphers dropbear-kex dropbear-server
#INTEROP_TESTS+=ssh-com ssh-com-client ssh-com-keygen ssh-com-sftp

EXTRA_TESTS=	agent-pkcs11
#EXTRA_TESTS+= 	cipher-speed

USERNAME=		${LOGNAME}
CLEANFILES=	*.core actual agent-key.* authorized_keys_${USERNAME} \
		authorized_keys_${USERNAME}.* \
		authorized_principals_${USERNAME} \
		banner.in banner.out cert_host_key* cert_user_key* \
		copy.1 copy.2 data ed25519-agent ed25519-agent* \
		ed25519-agent.pub ed25519 ed25519.pub empty.in \
		expect failed-regress.log failed-ssh.log failed-sshd.log \
		hkr.* host.ecdsa-sha2-nistp256 host.ecdsa-sha2-nistp384 \
		host.ecdsa-sha2-nistp521 host.ssh-ed25519 \
		host.ssh-rsa host_ca_key* host_krl_* host_revoked_* key.* \
		key.ecdsa-* key.ed25519-512 \
		key.ed25519-512.pub key.rsa-* keys-command-args kh.* askpass \
		known_hosts known_hosts-cert known_hosts.* krl-* ls.copy \
		modpipe netcat no_identity_config \
		pidfile putty.rsa2 ready regress.log remote_pid \
		revoked-* rsa rsa-agent rsa-agent.pub rsa.pub rsa_ssh2_cr.prv \
		rsa_ssh2_crnl.prv scp-ssh-wrapper.exe \
		scp-ssh-wrapper.scp setuid-allowed sftp-server.log \
		sftp-server.sh sftp.log ssh-log-wrapper.sh ssh.log \
		ssh-agent.log ssh-add.log slow-sftp-server.sh \
		ssh-rsa_oldfmt knownhosts_command \
		ssh_config ssh_config.* ssh_proxy ssh_proxy_bak \
		ssh_proxy_* sshd.log sshd_config sshd_config.* \
		sshd_config.* sshd_proxy sshd_proxy.* sshd_proxy_bak \
		sshd_proxy_orig t10.out t10.out.pub t12.out t12.out.pub \
		t2.out t3.out t6.out1 t6.out2 t7.out t7.out.pub \
		t8.out t8.out.pub t9.out t9.out.pub \
		timestamp testdata user_*key* user_ca* user_key*

# Enable all malloc(3) randomisations and checks
TEST_ENV=      "MALLOC_OPTIONS=CFGJRSUX"

TEST_SSH_SSHKEYGEN?=ssh-keygen

CPPFLAGS=-I..

t1:
	set -xe ; if ${TEST_SSH_SSH} -Q key | grep -q "^ssh-rsa" ; then \
		${TEST_SSH_SSHKEYGEN} -if ${.CURDIR}/rsa_ssh2.prv | diff - ${.CURDIR}/rsa_openssh.prv ; \
		tr '\n' '\r' <${.CURDIR}/rsa_ssh2.prv > ${.OBJDIR}/rsa_ssh2_cr.prv ; \
		${TEST_SSH_SSHKEYGEN} -if ${.OBJDIR}/rsa_ssh2_cr.prv | diff - ${.CURDIR}/rsa_openssh.prv ; \
		awk '{print $$0 "\r"}' ${.CURDIR}/rsa_ssh2.prv > ${.OBJDIR}/rsa_ssh2_crnl.prv ; \
		${TEST_SSH_SSHKEYGEN} -if ${.OBJDIR}/rsa_ssh2_crnl.prv | diff - ${.CURDIR}/rsa_openssh.prv ; \
	fi

t2:
	set -xe ; if ${TEST_SSH_SSH} -Q key | grep -q "^ssh-rsa" ; then \
		cat ${.CURDIR}/rsa_openssh.prv > $(OBJ)/t2.out ; \
		chmod 600 $(OBJ)/t2.out ; \
		${TEST_SSH_SSHKEYGEN} -yf $(OBJ)/t2.out | diff - ${.CURDIR}/rsa_openssh.pub ; \
	fi

t3:
	set -xe ; if ${TEST_SSH_SSH} -Q key | grep -q "^ssh-rsa" ; then \
		${TEST_SSH_SSHKEYGEN} -ef ${.CURDIR}/rsa_openssh.pub >$(OBJ)/t3.out ; \
		${TEST_SSH_SSHKEYGEN} -if $(OBJ)/t3.out | diff - ${.CURDIR}/rsa_openssh.pub ; \
	fi

t4:
	set -xe ; if ${TEST_SSH_SSH} -Q key | grep -q "^ssh-rsa" ; then \
		${TEST_SSH_SSHKEYGEN} -E md5 -lf ${.CURDIR}/rsa_openssh.pub |\
			awk '{print $$2}' | diff - ${.CURDIR}/t4.ok ; \
	fi

t5:
	set -xe ; if ${TEST_SSH_SSH} -Q key | grep -q "^ssh-rsa" ; then \
		${TEST_SSH_SSHKEYGEN} -Bf ${.CURDIR}/rsa_openssh.pub |\
			awk '{print $$2}' | diff - ${.CURDIR}/t5.ok ; \
	fi

$(OBJ)/t7.out:
	set -xe ; if ${TEST_SSH_SSH} -Q key | grep -q "^ssh-rsa" ; then \
		${TEST_SSH_SSHKEYGEN} -q -t rsa -N '' -f $@ ; \
	fi

t7: $(OBJ)/t7.out
	set -xe ; if ${TEST_SSH_SSH} -Q key | grep -q "^ssh-rsa" ; then \
		${TEST_SSH_SSHKEYGEN} -lf $(OBJ)/t7.out > /dev/null ; \
		${TEST_SSH_SSHKEYGEN} -Bf $(OBJ)/t7.out > /dev/null ; \
	fi

$(OBJ)/t9.out:
	! ${TEST_SSH_SSH} -Q key-plain | grep ecdsa >/dev/null || \
	${TEST_SSH_SSHKEYGEN} -q -t ecdsa -N '' -f $@

t9: $(OBJ)/t9.out
	! ${TEST_SSH_SSH} -Q key-plain | grep ecdsa >/dev/null || \
	${TEST_SSH_SSHKEYGEN} -lf $(OBJ)/t9.out > /dev/null
	! ${TEST_SSH_SSH} -Q key-plain | grep ecdsa >/dev/null || \
	${TEST_SSH_SSHKEYGEN} -Bf $(OBJ)/t9.out > /dev/null


$(OBJ)/t10.out:
	${TEST_SSH_SSHKEYGEN} -q -t ed25519 -N '' -f $@

t10: $(OBJ)/t10.out
	${TEST_SSH_SSHKEYGEN} -lf $(OBJ)/t10.out > /dev/null
	${TEST_SSH_SSHKEYGEN} -Bf $(OBJ)/t10.out > /dev/null

t11:
	set -xe ; if ${TEST_SSH_SSH} -Q key | grep -q "^ssh-rsa" ; then \
		${TEST_SSH_SSHKEYGEN} -E sha256 -lf ${.CURDIR}/rsa_openssh.pub |\
			awk '{print $$2}' | diff - ${.CURDIR}/t11.ok ; \
	fi

$(OBJ)/t12.out:
	${TEST_SSH_SSHKEYGEN} -q -t ed25519 -N '' -C 'test-comment-1234' -f $@

t12: $(OBJ)/t12.out
	${TEST_SSH_SSHKEYGEN} -lf $(OBJ)/t12.out.pub | grep test-comment-1234 >/dev/null

t-exec:	${LTESTS:=.sh}
	@if [ "x$?" = "x" ]; then exit 0; fi; \
	_started=""; test -z "${LTESTS_FROM}" && _started=1 ;\
	for TEST in ""$?; do \
		if [ -z "$$_started" ] ; then \
			if [ "x$$TEST" = "x${LTESTS_FROM}.sh" ]; then \
				_started=1; \
			else \
				continue; \
			fi ; \
		fi ; \
		skip=no; \
		for t in ""$${SKIP_LTESTS}; do \
			if [ "x$${t}.sh" = "x$${TEST}" ]; then skip=yes; fi; \
		done; \
		if [ "x$${skip}" = "xno" ]; then \
			echo "run test $${TEST}" ... 1>&2; \
			(env SUDO="${SUDO}" TEST_ENV=${TEST_ENV} ${TEST_SHELL} ${.CURDIR}/test-exec.sh ${.OBJDIR} ${.CURDIR}/$${TEST}) || exit $$?; \
		else \
			echo skip test $${TEST} 1>&2; \
		fi; \
	done

t-exec-interop:	${INTEROP_TESTS:=.sh}
	@if [ "x$?" = "x" ]; then exit 0; fi; \
	for TEST in ""$?; do \
		echo "run test $${TEST}" ... 1>&2; \
		(env SUDO="${SUDO}" TEST_ENV=${TEST_ENV} ${TEST_SHELL} ${.CURDIR}/test-exec.sh ${.OBJDIR} ${.CURDIR}/$${TEST}) || exit $$?; \
	done

t-extra:	${EXTRA_TESTS:=.sh}
	@if [ "x$?" = "x" ]; then exit 0; fi; \
	for TEST in ""$?; do \
		echo "run test $${TEST}" ... 1>&2; \
		(env SUDO="${SUDO}" TEST_ENV=${TEST_ENV} ${TEST_SHELL} ${.CURDIR}/test-exec.sh ${.OBJDIR} ${.CURDIR}/$${TEST}) || exit $$?; \
	done

# Not run by default
interop: ${INTEROP_TARGETS}

# Unit tests, built by top-level Makefile
unit unit-bench:
	set -e ; if test -z "${SKIP_UNIT}" ; then \
		V="" ; \
		test "x${USE_VALGRIND}" = "x" || \
		    V=${.CURDIR}/valgrind-unit.sh ; \
		ARGS=""; \
		test "x$@" = "xunit-bench" && ARGS="-b"; \
		test "x${UNITTEST_FAST}" = "x" || ARGS="$$ARGS -f"; \
		test "x${UNITTEST_SLOW}" = "x" || ARGS="$$ARGS -F"; \
		test "x${UNITTEST_VERBOSE}" = "x" || ARGS="$$ARGS -v"; \
		test "x${UNITTEST_BENCH_DETAIL}" = "x" || ARGS="$$ARGS -B"; \
		test "x${UNITTEST_BENCH_ONLY}" = "x" || ARGS="$$ARGS -O ${UNITTEST_BENCH_ONLY}"; \
		 $$V ${.OBJDIR}/unittests/sshbuf/test_sshbuf $${ARGS}; \
		 $$V ${.OBJDIR}/unittests/sshkey/test_sshkey \
			-d ${.CURDIR}/unittests/sshkey/testdata $${ARGS}; \
		$$V ${.OBJDIR}/unittests/sshsig/test_sshsig \
			-d ${.CURDIR}/unittests/sshsig/testdata $${ARGS}; \
		$$V ${.OBJDIR}/unittests/authopt/test_authopt \
			-d ${.CURDIR}/unittests/authopt/testdata $${ARGS}; \
		$$V ${.OBJDIR}/unittests/bitmap/test_bitmap $${ARGS}; \
		$$V ${.OBJDIR}/unittests/conversion/test_conversion $${ARGS}; \
		$$V ${.OBJDIR}/unittests/kex/test_kex $${ARGS}; \
		$$V ${.OBJDIR}/unittests/hostkeys/test_hostkeys \
			-d ${.CURDIR}/unittests/hostkeys/testdata $${ARGS}; \
		$$V ${.OBJDIR}/unittests/match/test_match $${ARGS}; \
		$$V ${.OBJDIR}/unittests/misc/test_misc $${ARGS}; \
		if test "x${TEST_SSH_UTF8}" = "xyes"  ; then \
			$$V ${.OBJDIR}/unittests/utf8/test_utf8 $${ARGS}; \
		fi \
	fi
