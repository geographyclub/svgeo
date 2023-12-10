# svgeo

Geographic layers converted to svg format -- ready to use with any vector editing software.

## Downloads

### Natural Earth  
[ne_10m_populated_places.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_10m_populated_places.svg)  
[ne_10m_roads.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_10m_roads.svg)  
[ne_50m_admin_0_countries_lakes.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_50m_admin_0_countries_lakes.svg)  
[ne_50m_admin_0_map_subunits.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_50m_admin_0_map_subunits.svg)  
[ne_50m_geography_marine_polys.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_50m_geography_marine_polys.svg)  
[ne_50m_geography_regions_polys.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_50m_geography_regions_polys.svg)  

### Globes  
[hex1_ne_50m_land_-100_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_-100_-20.svg)
[hex1_ne_50m_land_100_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_100_-20.svg)
[hex1_ne_50m_land_-140_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_-140_-20.svg)
[hex1_ne_50m_land_140_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_140_-20.svg)
[hex1_ne_50m_land_-180_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_-180_-20.svg)
[hex1_ne_50m_land_180_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_180_-20.svg)
[hex1_ne_50m_land_-20_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_-20_-20.svg)
[hex1_ne_50m_land_20_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_20_-20.svg)
[hex1_ne_50m_land_-60_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_-60_-20.svg)
[hex1_ne_50m_land_60_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/hex1_ne_50m_land_60_-20.svg)
[ne_10m_graticules_1_split1_-100_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_-100_-20.svg)
[ne_10m_graticules_1_split1_100_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_100_-20.svg)
[ne_10m_graticules_1_split1_-140_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_-140_-20.svg)
[ne_10m_graticules_1_split1_140_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_140_-20.svg)
[ne_10m_graticules_1_split1_-180_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_-180_-20.svg)
[ne_10m_graticules_1_split1_180_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_180_-20.svg)
[ne_10m_graticules_1_split1_-20_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_-20_-20.svg)
[ne_10m_graticules_1_split1_20_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_20_-20.svg)
[ne_10m_graticules_1_split1_-60_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_-60_-20.svg)
[ne_10m_graticules_1_split1_60_-20.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ortho/ne_10m_graticules_1_split1_60_-20.svg)

## Conversions

Convert layer with name.  
```bash
file=natural_earth_vector.gpkg
layer=ne_10m_populated_places
width=1920
height=960

ogrinfo -dialect sqlite -sql "SELECT ST_MinX(extent(geom)) || CAST(X'09' AS TEXT) || (-1 * ST_MaxY(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxX(extent(geom)) - ST_MinX(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxY(extent(geom)) - ST_MinY(extent(geom))) || CAST(X'09' AS TEXT) || GeometryType(geom) FROM '"${layer}"'" ${file} | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
  echo '<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="'${height}'" width="'${width}'" viewBox="'${array[0]}' '${array[1]}' '${array[2]}' '${array[3]}'">' > ~/svgeo/svg/${layer}.svg
  case ${array[4]} in
    POINT|MULTIPOINT)
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || ST_X(ST_Centroid(geom)) || CAST(X'09' AS TEXT) || (-1 * ST_Y(ST_Centroid(geom))) || CAST(X'09' AS TEXT) || REPLACE(name,'&','and') FROM ${layer} WHERE geom NOT LIKE '%null%'" ${file} | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<circle id="'${array[0]}'" cx="'${array[1]}'" cy="'${array[2]}'" r="0.6px" vector-effect="non-scaling-stroke" fill="#000" fill-opacity="1" stroke="#000" stroke-width="0px" stroke-linejoin="round" stroke-linecap="round"><title>'${array[3]}'</title></circle>' >> ~/svgeo/svg/${layer}.svg
      done
      ;;
    LINESTRING|MULTILINESTRING)
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || 'M ' || ST_X(StartPoint(geom)) || ' ' || (-1 * ST_Y(StartPoint(geom))) || 'L ' || ST_X(EndPoint(geom)) || ' ' || (-1 * ST_Y(EndPoint(geom))) || CAST(X'09' AS TEXT) || REPLACE(name,'&','and') FROM ${layer} WHERE geom NOT LIKE '%null%'" ${file} | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" stroke="#000" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round" fill="none"><title>'${array[2]}'</title></path>' >> ~/svgeo/svg/${layer}.svg
      done
      ;;
    POLYGON|MULTIPOLYGON)
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || AsSVG(geom, 1) || CAST(X'09' AS TEXT) || REPLACE(name,'&','and') FROM ${layer} WHERE geom NOT LIKE '%null%'" ${file} | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" fill="#000" fill-opacity="1" stroke="#FFF" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round"><title>'${array[2]}'</title></path>' >> ~/svgeo/svg/${layer}.svg
      done
      ;;
  esac
  echo '</svg>' >> ~/svgeo/svg/${layer}.svg
done
```

Convert layer with filter.  
```bash
file=natural_earth_vector.gpkg
layer=ne_10m_roads
where="scalerank IN (3,4,5)"
width=1920
height=960

ogrinfo -dialect sqlite -sql "SELECT ST_MinX(extent(geom)) || CAST(X'09' AS TEXT) || (-1 * ST_MaxY(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxX(extent(geom)) - ST_MinX(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxY(extent(geom)) - ST_MinY(extent(geom))) || CAST(X'09' AS TEXT) || GeometryType(geom) FROM '"${layer}"'" ${file} | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
  echo '<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="'${height}'" width="'${width}'" viewBox="'${array[0]}' '${array[1]}' '${array[2]}' '${array[3]}'">' > ~/svgeo/svg/${layer}.svg
  case ${array[4]} in
    POINT|MULTIPOINT)
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || ST_X(ST_Centroid(geom)) || CAST(X'09' AS TEXT) || (-1 * ST_Y(ST_Centroid(geom))) FROM ${layer} WHERE geom NOT LIKE '%null%'" ${file} | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<circle id="'${array[0]}'" cx="'${array[1]}'" cy="'${array[2]}'" r="0.6px" vector-effect="non-scaling-stroke" fill="#FFF" fill-opacity="1" stroke="#000" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round"><title></title></circle>' >> ~/svgeo/svg/${layer}.svg
      done
      ;;
    LINESTRING|MULTILINESTRING)
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || 'M ' || ST_X(StartPoint(geom)) || ' ' || (-1 * ST_Y(StartPoint(geom))) || 'L ' || ST_X(EndPoint(geom)) || ' ' || (-1 * ST_Y(EndPoint(geom))) FROM ${layer} WHERE geom NOT LIKE '%null%' AND $(echo ${where})" ${file} | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" stroke="#000" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round" fill="none"><title></title></path>' >> ~/svgeo/svg/${layer}.svg
      done
      ;;
    POLYGON|MULTIPOLYGON)
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || AsSVG(geom, 1) FROM ${layer} WHERE geom NOT LIKE '%null%'" ${file} | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" fill="#000" fill-opacity="1" stroke="#FFF" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round"><title></title></path>' >> ~/svgeo/svg/${layer}.svg
      done
      ;;
  esac
  echo '</svg>' >> ~/svgeo/svg/${layer}.svg
done
```

Convert postgis table with intersection.  
```bash
layer=wwf_terr_ecos
continent="Asia"
width=1920
height=960

psql -d world -c "COPY (SELECT ST_XMin(ST_Extent(geom)), (-1 * ST_YMax(ST_Extent(geom))), (ST_XMax(ST_Extent(geom)) - ST_XMin(ST_Extent(geom))), (ST_YMax(ST_Extent(geom)) - ST_YMin(ST_Extent(geom))), (SELECT GeometryType(wkb_geometry) FROM ${layer} LIMIT 1) FROM ne_10m_continents WHERE continent = '${continent}') TO STDOUT DELIMITER E'\t'" | while IFS=$'\t' read -a array; do
  echo '<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="'${height}'" width="'${width}'" viewBox="'${array[0]}' '${array[1]}' '${array[2]}' '${array[3]}'">' > ~/svgeo/svg/${layer}_${continent// /}.svg
  case ${array[4]} in
    POINT|MULTIPOINT)
      psql -d world -c "COPY (WITH clip AS (SELECT b.fid, ST_Intersection(a.wkb_geometry, b.geom) geom FROM ${layer} a, ne_10m_continents b WHERE b.continent = '${continent}' AND ST_Intersects(a.wkb_geometry, b.geom)) SELECT fid, ST_X(ST_Centroid(geom)), (-1 * ST_Y(ST_Centroid(geom))) FROM clip) TO STDOUT DELIMITER E'\t'" | while IFS=$'\t' read -a array; do
        echo '<circle id="'${array[0]}'" cx="'${array[1]}'" cy="'${array[2]}'" r="1em" vector-effect="non-scaling-stroke" fill="#FFF" fill-opacity="1" stroke="#000" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round"><title></title></circle>' >> ~/svgeo/svg/${layer}_${continent// /}.svg
      done
      ;;
    LINESTRING|MULTILINESTRING)
      psql -d world -c "COPY (WITH clip AS (SELECT b.fid, ST_Intersection(a.wkb_geometry, b.geom) geom FROM ${layer} a, ne_10m_continents b WHERE b.continent = '${continent}' AND ST_Intersects(a.wkb_geometry, b.geom)) SELECT fid, 'M ' || ST_X(StartPoint(geom)) || ' ' || (-1 * ST_Y(StartPoint(geom))) || 'L ' || ST_X(EndPoint(geom)) || ' ' || (-1 * ST_Y(EndPoint(geom))) FROM clip) TO STDOUT DELIMITER E'\t'" | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" stroke="#000" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round" fill="none"><title></title></path>' >> ~/svgeo/svg/${layer}_${continent// /}.svg
      done
      ;;
    POLYGON|MULTIPOLYGON)
      psql -d world -c "COPY (WITH clip AS (SELECT b.fid, ST_Intersection(a.wkb_geometry, b.geom) geom FROM ${layer} a, ne_10m_continents b WHERE b.continent = '${continent}' AND ST_Intersects(a.wkb_geometry, b.geom)) SELECT fid, ST_AsSVG(geom, 1) FROM clip) TO STDOUT DELIMITER E'\t'" | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" fill="#000" fill-opacity="1" stroke="#FFF" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round"><title></title></path>' >> ~/svgeo/svg/${layer}_${continent// /}.svg
      done
      ;;
  esac
  echo '</svg>' >> ~/svgeo/svg/${layer}_${continent// /}.svg
done
```

Convert multiple layers into one svg.  
```bash
# make ortho layers
file='/home/steve/maps/grids/hex1_ne_50m_land.gpkg'
layer='hex1_ne_50m_land'
for x in $(seq -180 40 180); do
  for y in '-20'; do
    proj='+proj=ortho +lat_0='"${y}"' +lon_0='"${x}"' +ellps=sphere'
    ogr2ogr -overwrite -skipfailures --config OGR_ENABLE_PARTIAL_REPROJECTION TRUE -s_srs 'epsg:4326' -t_srs "${proj}" ${layer}_${x}_${y}.gpkg ${file} ${layer}
  done
done

# union
file='/home/steve/maps/grids/hex1_ne_50m_land.gpkg'
layer='hex1_ne_50m_land'
for x in $(seq -180 40 180); do
  for y in '-20'; do
    proj='+proj=ortho +lat_0='"${y}"' +lon_0='"${x}"' +ellps=sphere'
    ogr2ogr -overwrite -skipfailures --config OGR_ENABLE_PARTIAL_REPROJECTION TRUE -s_srs 'epsg:4326' -t_srs "${proj}" -f 'GeoJSON' /vsistdout/ ${file} ${layer} | ogr2ogr -overwrite -skipfailures --config OGR_ENABLE_PARTIAL_REPROJECTION TRUE -dialect 'SQLite' -sql "SELECT ST_Union(ST_Buffer(geometry,0.1)) AS geom FROM ${layer}" -nln ${layer} ${layer}_${x}_${y}.gpkg /vsistdin/
  done
done

# make svg (graticules + boundary + coastline)
layer1='ne_50m_admin_0_boundary_lines_land_split1'
layer2='ne_50m_coastline_split1'
layer3='ne_10m_graticules_1_split1'
height=540
width=540
for x in $(seq -180 40 180); do
  for y in '-20'; do
    ogrinfo -dialect sqlite -sql "SELECT ST_MinX(extent(geom)) || CAST(X'09' AS TEXT) || (-1 * ST_MaxY(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxX(extent(geom)) - ST_MinX(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxY(extent(geom)) - ST_MinY(extent(geom))) FROM ${layer3}" ${layer3}_-100_-20.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
      echo '<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="'${height}'" width="'${width}'" viewBox="'${array[0]}' '${array[1]}' '${array[2]}' '${array[3]}'">' > ${layer1}_${x}_${y}.svg
      # layer 1
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || 'M ' || ST_X(StartPoint(geom)) || ' ' || (-1 * ST_Y(StartPoint(geom))) || 'L ' || ST_X(EndPoint(geom)) || ' ' || (-1 * ST_Y(EndPoint(geom))) FROM ${layer1} WHERE geom NOT LIKE '%null%'" ${layer1}_${x}_${y}.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" stroke="#000" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round" fill="none"></path>' >> ${layer1}_${x}_${y}.svg
      done
      # layer 2
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || 'M ' || ST_X(StartPoint(geom)) || ' ' || (-1 * ST_Y(StartPoint(geom))) || 'L ' || ST_X(EndPoint(geom)) || ' ' || (-1 * ST_Y(EndPoint(geom))) FROM ${layer2} WHERE geom NOT LIKE '%null%'" ${layer2}_${x}_${y}.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" stroke="#000" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round" fill="none"></path>' >> ${layer1}_${x}_${y}.svg
      done
      # layer 3
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || 'M ' || ST_X(StartPoint(geom)) || ' ' || (-1 * ST_Y(StartPoint(geom))) || 'L ' || ST_X(EndPoint(geom)) || ' ' || (-1 * ST_Y(EndPoint(geom))) FROM ${layer3} WHERE geom NOT LIKE '%null%' AND degrees LIKE '%0' OR degrees IN ('0')" ${layer3}_${x}_${y}.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" stroke="#000" stroke-width="0.2px" stroke-linejoin="round" stroke-linecap="round" fill="none"></path>' >> ${layer1}_${x}_${y}.svg
      done
      echo '</svg>' >> ${layer1}_${x}_${y}.svg
    done
  done
done

# make svg (graticules + hex1)
layer1='hex1_ne_50m_land'
layer2='ne_10m_graticules_1_split1'
height=540
width=540
for x in $(seq -180 40 180); do
  for y in '-20'; do
    ogrinfo -dialect sqlite -sql "SELECT ST_MinX(extent(geom)) || CAST(X'09' AS TEXT) || (-1 * ST_MaxY(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxX(extent(geom)) - ST_MinX(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxY(extent(geom)) - ST_MinY(extent(geom))) FROM ${layer1}" ${layer1}_-100_-20.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
      echo '<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="'${height}'" width="'${width}'" viewBox="'${array[0]}' '${array[1]}' '${array[2]}' '${array[3]}'">' > ${layer1}_${x}_${y}.svg
      # layer 1
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || AsSVG(geom, 1) FROM ${layer1} WHERE geom NOT LIKE '%null%'" ${layer1}_${x}_${y}.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" fill="#000" fill-opacity="1" stroke="#000" stroke-width="0" stroke-linejoin="round" stroke-linecap="round"><title>'${array[2]}'</title></path>' >>  ${layer1}_${x}_${y}.svg
      done
      # layer 2
      ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || 'M ' || ST_X(StartPoint(geom)) || ' ' || (-1 * ST_Y(StartPoint(geom))) || 'L ' || ST_X(EndPoint(geom)) || ' ' || (-1 * ST_Y(EndPoint(geom))) FROM ${layer2} WHERE geom NOT LIKE '%null%' AND degrees LIKE '%0' OR degrees IN ('0')" ${layer2}_${x}_${y}.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
        echo '<path id="'${array[0]}'" d="'${array[1]}'" vector-effect="non-scaling-stroke" stroke="#000" stroke-width="0.2px" stroke-linejoin="round" stroke-linecap="round" fill="none"></path>' >> ${layer1}_${x}_${y}.svg
      done
      echo '</svg>' >> ${layer1}_${x}_${y}.svg
    done
  done
done
```
