build: build_dependencies
	docker build . -t hydra_ci

build_dependencies: Dockerfile cardano-node.tar.gz ghcup

cardano-node.tar.gz:
	curl https://update-cardano-mainnet.iohk.io/cardano-node-releases/cardano-node-1.35.7-linux.tar.gz -o cardano-node.tar.gz

ghcup:
	curl https://downloads.haskell.org/~ghcup/x86_64-linux-ghcup -o ghcup

clean:
	-rm cardano-node.tar.gz ghcup

.phony: build clean
