APP      = gview
STACK    = 4480k
MALLOC   = 0k

TOOLPATH = ../../../z_tools/
INCPATH  = ../../includes/
APILIBPATH   = ../../libs/apilib/
KernelPATH = ../../kernel/

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
CC1      = $(TOOLPATH)cc1.exe -I$(INCPATH) -I../ -Os -Wall -quiet
GAS2NASK = $(TOOLPATH)gas2nask.exe -a
OBJ2BIM  = $(TOOLPATH)obj2bim.exe
MAKEFONT = $(TOOLPATH)makefont.exe
BIN2OBJ  = $(TOOLPATH)bin2obj.exe
BIM2HRB  = $(TOOLPATH)bim2hrb.exe
BIM2BIN  = $(TOOLPATH)bim2bin.exe
RULEFILE = ../../ruls/zbote_ap.rul
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
GOLIB    = $(TOOLPATH)golib00.exe 
COPY     = copy
DEL      = del

# �f�t�H���g����

default :
	$(MAKE) $(APP).hrb

# �t�@�C�������K��

$(APP).bim : $(APP).obj bmp.obj jpeg.obj $(APILIBPATH)apilib.lib Makefile
	$(OBJ2BIM) @$(RULEFILE) out:$(APP).bim map:$(APP).map stack:$(STACK) \
		$(APP).obj jpeg.obj bmp.obj $(APILIBPATH)apilib.lib

zbote.img : ../kernel/ipl09.bin ../kernel/zbote.sys $(APP).hrb \
		Makefile
	$(EDIMG)   imgin:../../../z_tools/fdimg0at.tek \
		wbinimg src:../../kernel/ipl09.bin len:512 from:0 to:0 \
		copy from:../../kernel/zbote.sys to:@: \
		copy from:$(APP).hrb to:@: \
		copy from:../../fonts/chinese.fnt to:@: \
		imgout:zbote.img

# ��ʋK��

%.gas : %.c ../../libs/apilib.h Makefile
	$(CC1) -o $*.gas $*.c

%.nas : %.gas Makefile
	$(GAS2NASK) $*.gas $*.nas

%.obj : %.nas Makefile
	$(NASK) $*.nas $*.obj $*.lst

%.org : %.bim Makefile
	$(BIM2HRB) $*.bim $*.org $(MALLOC)

%.hrb : %.org Makefile
	$(BIM2BIN) -osacmp in:$*.org out:$*.hrb

# �R�}���h

run :
	$(MAKE) zbote.img
	$(COPY) zbote.img ..\..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ../../../z_tools/qemu

full :
	$(MAKE) -C $(APILIBPATH)
	$(MAKE) $(APP).hrb

run_full :
	$(MAKE) -C $(APILIBPATH)
	$(MAKE) -C ../../kernel
	$(MAKE) run

clean :
	-$(DEL) *.lst
	-$(DEL) gview.obj
	-$(DEL) jpeg.obj
	-$(DEL) *.map
	-$(DEL) *.bim
	-$(DEL) *.org
	-$(DEL) zbote.img

src_only :
	$(MAKE) clean
	-$(DEL) $(APP).hrb
