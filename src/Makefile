PKG = fsboost
PREFIX = $(shell pkg-config --variable=prefix gretl)

INP = $(PKG).inp $(PKG)_sample.inp
GFN = $(PKG).gfn

$(PKG).gfn : $(INP) $(PKG).spec
	gretlcli --makepkg $(PKG).inp

$(PKG).zip : $(INP) $(GFN) copy_pdf
	echo "makepkg $(PKG).zip" | gretlcli -b -

copy_pdf : ../docs/$(PKG).pdf
	cp -r ../docs/$(PKG).pdf ./

install: $(PKG).zip
	echo "pkg install $(PKG).zip --local" | gretlcli -b -

tarball : clean
	tar -cvzf ../$(PKG).zip *

clean:
	rm -rf $(GFN) $(PKG).zip
	rm -rf session.inp
	rm -rf ./$(PKG).pdf
