
  1. About

  HimemX is a XMS memory manager. It's derived from FreeDOS Himem 
  (short: FDHimem) with bugfixes, optimizations and extensions.
  To see its usage just run HIMEMX.EXE

  UMBM is a small tool which, if used together with UMBPCI, might allow
  to load HimemX in DOS upper memory. To see its usage just run UMBM.EXE


  2. History

  02/01/2020, v3.34:
  - the amount of extended memory restricted by /MAX doesn't include the
    HMA.
  - multiple free memory blocks reported by int 15h, ax=e820h can now be
    handled.

  --/--/2009, v3.33:


  03/13/2008, v3.32:
  - the amount of extended memory restricted by /MAX does now include the
    HMA. That means, a /MAX=2048k parameter restricts Himem to exactly 
    use range 0x100000-0x2FFFFF.
  - TASM compatibility abandoned (didn't work with v3.31 either). Instead
    source is now compatible with JWasm.
  - binary size reduction.

  07/23/2007, v3.31:
  - bugfix: for MS-DOS v6.22 and below, HimemX damaged parts of its code
    during initialization.
  - bugfix: after PE bit is set on a 80386/80486, there must be at least
    1 instruction before a segment register can be set. This wasn't true
    in v3.30 using unreal mode.

  04/29/2007: v3.30 
  - binary size reduction

  04/19/2007, v3.29:
  - bugfix: in XMS functions 08h/88h the BH register was modified.
  - bugfix: global disable A20 does no longer disable A20 if the local 
    disable counter is > 0.
  - UMBM.EXE added    

  04/13/2007, v3.28:
  - bugfix: to mark unused handles FDHimem moves 01 ("free") into the flag
    field of the XMS handle and clears base address and size. HimemX
    marks unused handles by moving 04 ("unallocated") into the flag field,
    which is the - documented - MS Himem way to do it.
  - bugfix: FDHimem's "block move" implementation unconditionally enables
    interrupts if cpu is in v86-mode. HimemX never enables interrupts if
    IF is cleared on entry.
  - bugfix: the "resize EMB" function (AH=0Fh/8Fh) of FDHimem is not 
    reentrant.
  - bugfix: in real-mode, FDHimem switches to protected mode and sets DS,ES
    to a flat, 4GB data selector to move to/from extended memory. After the
    move DS and ES are not reset to a "standard" segment with FFFFh limit.
    This in fact enables and keeps "unreal mode" unintentionally.
  - bugfix: for XMS local disable A20 function, FDHimem does not test an
    underflow condition of the counter (0 -> -1)
  - bugfix: HimemX checks for a 80386 cpu *before* using 32bit opcodes, so
    there is a true chance that this driver will not crash on a 80286.
  - bugfix: FDHimem's hook code for int 15h, ah=87h does not return reported 
    status of carry flag to the caller.
  - bugfix: FDHimem's implementation of function 0E (get handle info) might
    have returned 0 for block size if block size was indeed exactly 64 MB.
  - bugfix: FDHimem opens an "interrupt window" in v86-mode only. HimemX opens
    an "interrupt window" in both v86- and real-mode.
  - bugfix: if no extended memory is free, FDHimem doesn't allow to allocate
    an EMB with size 0. MS Windows 9x needs this feature.
  - in FDHimem the routine which tests whether A20 is enabled is more
    complicated than necessary.
  - in FDHimem the code which switches to protected-mode and back 
    sets/restores CS which is not necessary.
  - some variables which don't have to be resident were moved to the _DATA
    section, thus reducing size of the resident part.
  - HimemX is written completely in ASM, the C part has been abandoned.
  - unused code in resident part has been deactivated (proc delay2)
  - default number of handles in HimemX is 48, in FDHimem it is 72.
  - /X2MAX32 switch is off by default in HimemX, it is on in FDHimem.
  - if extended memory is >= 64 MB, the size returned by XMS V2 function
    is 65535 kB in HimemX, FDHimem returns 64448 kB.
  - generally, the source was changed from TASM IDEAL to MASM format.
    It still can be assembled by TASM, though. OW's WASM cannot be used.
  - HimemX uses "unreal" mode for moving EMBs if cpu is not in v86-mode.


  3. License
  
  All changes done for HimemX are Public Domain. However, since HimemX is
  derived from FD Himem, the FD Himem copyrights do apply. FD Himem is
  copyright Till Gerken and Tom Ehlert, with GPL and/or Artistic license.

  UMBM.EXE is Public Domain. The source code UMBM.ASM is supplied with
  Jemm.

  Japheth
