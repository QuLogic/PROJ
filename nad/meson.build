# PROJ.4 test scripts
TEST27 = find_program('test27')
TEST83 = find_program('test83')
TESTNTV2 = find_program('testntv2')
TESTVARIOUS = find_program('testvarious')
TESTFLAKY = find_program('testflaky')
TESTDATUMFILE = find_program('testdatumfile')
TESTIGN = find_program('testIGNF')

install_data(
    'GL27',
    'nad.lst',
    'proj_def.dat',
    'nad27',
    'nad83',
    'world',
    'epsg',
    'esri',
    'esri.extra',
    'other.extra',
    'CH',
    'IGNF',
    'ITRF2000',
    'ITRF2008',
    'ITRF2014',
    install_dir : pkgdatadir)

foreach nad : []#['null']  # 'conus'
    custom_target(nad,
        input : '@0@.lla'.format(nad),
        output : nad,
        command : [nad2bin, '@INPUT@', '@OUTPUT@'],
        install : true,
        install_dir : pkgdatadir)
endforeach

install_data(
    'ntf_r93.gsb',
    'ntv1_can.dat',
    'nzgd2kgrid0005.gsb',
    install_dir : pkgdatadir)

test('test27', TEST27, args : [proj])
test('test83', TEST83, args : [proj])

env = environment()
env.set('PROJ_LIB', '.')
test('testvarious', TESTVARIOUS, args : [cs2cs], env : env)
test('testign', TESTIGN, args : [cs2cs], env : env)

#check-local: process-nad2bin
## Check if we are doing an out-of-tree build
#	@if test ! -f epsg; then \
#	  for gridfile in $(NADPATH)/*.gsb $(NADPATH)/ntv1_can.dat dummy ; do \
#	    if test "$$gridfile" != "dummy" -a -f "$$gridfile" ; then \
#	      cp $$gridfile .; \
#	    fi; \
#	  done; \
#	  for gridfile in $(pkgdata_DATA); do \
#	    cp $(NADPATH)/$$gridfile .; \
#	  done; \
#	fi
#	@if [ -f ntv2_0.gsb ] ; then \
#	  export PROJ_LIB=. ; \
#	  $(TESTNTV2) $(CS2CSEXE) ; \
#	fi
