include Makefile.rule

OCAMLFIND_PACKAGES+= str

OUT=main
GENERATED_FILES=

.PHONY: clean all generate app
.DEFAULT_GOAL: all
all: $(OUT)

LABLQML=../../lib
CLINK=g++ -g  -fPIC
CLINKLIBS=
CMX=event_emitter.cmx
CMX_TEST=


$(OUT): $(GENERATED_H) $(GENERATED_OBJS) $(GENERATED_CMX) $(CMX) camlcode.o  main.o
	$(V)$(CLINK) -L`ocamlc -where` \
	$(GENERATED_OBJS) camlcode.o main.o -lasmrun \
	-L$(LABLQML_WHERE) \
	-L`ocamlfind query threads` \
	-L`ocamlfind query lwt.unix` \
	-llwt_unix_stubs -lunix -lcamlstr -lthreadsnat -lpthread \
	$(CLINKLIBS) $(NATIVECCLIBS)  -o $(OUT)

camlcode.o: $(GEN_CMX) $(CMX)
	$(V)PATH=../../src/:$$PATH OCAMLPATH=../../lib/_build/bundle \
	$(OCAMLOPT) -output-obj -dstartup \
	$(GENERATED_CMX) $(CMX) -linkpkg -linkall -o camlcode.o

main.o: CXXFLAGS += -Dprotected=public
main.o: CXXFLAGS += -I$(LABLQML_WHERE) #for lablqml.h
main.o: main.cpp
	$(V)$(CXX) $(CXXFLAGS) -I`ocamlfind c -where` -c $< -o $@

.ml.cmx:
	$(V)PATH=../../src/:$$PATH OCAMLPATH=../../lib/_build/bundle \
	$(OCAMLOPT)  -c $<

clean::
	$(RM) *.o *.cm[oiax] *.cmxa *.o.startup.s $(MOC_CPP) $(GEN_H) $(GEN_CPP) $(OUT)
