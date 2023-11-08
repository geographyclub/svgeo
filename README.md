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

## Editing

Merging svg files.  
```bash
TODO
```

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
