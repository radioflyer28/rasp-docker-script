FROM yavalek/drjack-wrf3:latest

ENV BASEDIR=/root/rasp/

WORKDIR /root/

ADD geog.tar.gz $BASEDIR

# updated hi res data
ADD greenfrac_fpar_modis.tar.bz2 \
  bnu_soiltype_bot.tar.bz2 \
  bnu_soiltype_top.tar.bz2 \
  modis_landuse_20class_15s.tar.bz2 \
  topo_SRTM3S.tar.gz $BASEDIR/geog/

RUN cp -a $BASEDIR/region.TEMPLATE $BASEDIR/APLCN_B

COPY APLCN_B/wrfsi.nl APLCN_B/rasp.run.parameters.APLCN_B $BASEDIR/APLCN_B/
COPY APLCN_B/GEOGRID.TBL $BASEDIR/RUN.TABLES/
COPY APLCN_B/rasp.region_data.ncl $BASEDIR/GM/
COPY APLCN_B/rasp.site.runenvironment $BASEDIR/

ENV PATH="${BASEDIR}/bin:${PATH}"

# initialize
RUN cd $BASEDIR/APLCN_B/ \
  && wrfsi2wps.pl \
  && wps2input.pl
  
# overwrite customized files
COPY APLCN_B/namelist.input.template APLCN_B/namelist.wps.template $BASEDIR/APLCN_B/
COPY APLCN_B/namelist.wps.template $BASEDIR/APLCN_B/namelist.wps

RUN cd $BASEDIR/APLCN_B/ \
  && geogrid.exe

# cleanup
RUN rm -rf $BASEDIR/geog
RUN rm -rf PANOCHE

WORKDIR /root/rasp/

VOLUME ["/root/rasp/APLCN_B/OUT/", "/root/rasp/APLCN_B/LOG/"]

CMD ["runGM", "APLCN_B"]
