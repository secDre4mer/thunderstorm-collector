ALL_TARGETS := $(shell go tool dist list)

ALLARCHS := $(sort $(subst /,,$(notdir $(ALL_TARGETS))))
ALLOS := $(sort $(subst /,,$(dir $(ALL_TARGETS))))

BLACKLIST_OS := android ios

all: $(filter-out $(addsuffix %,$(BLACKLIST_OS)),$(ALL_TARGETS))

$(foreach arch, $(ALLARCHS), \
	$(foreach os, $(ALLOS), \
		$(eval bin/$(arch)-$(os)-%: GOARCH=$(arch)) \
		$(eval bin/$(arch)-$(os)-%: GOOS=$(os)) \
		$(eval .PHONY: $(os)/$(arch)) \
		$(eval $(os)/$(arch): bin/$(arch)-$(os)-thunderstorm-collector$(if $(findstring windows,$(os)),.exe,)) \
))

BUILD_ENV=GOOS=$(GOOS) GOARCH=$(GOARCH) CGO_ENABLED=0

bin/%: $(wildcard *.go)
	$(BUILD_ENV) go build -o $@ -ldflags "-w -s"

test:
	go test -cover ./...

clean:
	rm -r bin

.PHONY: all clean
