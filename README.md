# svgeo

Geographic layers converted to svg format -- ready to use with any vector editing software.

## Downloads

### Natural Earth  
[ne_50m_admin_0_countries_lakes.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_50m_admin_0_countries_lakes.svg)
[ne_50m_admin_0_map_subunits.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_50m_admin_0_map_subunits.svg)
[ne_50m_geography_marine_polys.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_50m_geography_marine_polys.svg)
[ne_50m_geography_regions_polys.svg](https://github.com/geographyclub/svgeo/tree/main/svg/ne_50m_geography_regions_polys.svg)

## Conversion

Convert vector layer to svg file using GDAL/OGR shell script.  
```bash
# layer with name
layer=ne_50m_geography_regions_polys
width=1920
height=960

ogrinfo -dialect sqlite -sql "SELECT ST_MinX(extent(geom)) || CAST(X'09' AS TEXT) || (-1 * ST_MaxY(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxX(extent(geom)) - ST_MinX(extent(geom))) || CAST(X'09' AS TEXT) || (ST_MaxY(extent(geom)) - ST_MinY(extent(geom))) FROM '"${layer}"'" natural_earth_vector.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
  echo '<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" height="'${height}'" width="'${width}'" viewBox="'${array[0]}' '${array[1]}' '${array[2]}' '${array[3]}'">' > svg/${layer}.svg
  ogrinfo -dialect sqlite -sql "SELECT fid || CAST(X'09' AS TEXT) || ST_X(ST_Centroid(geom)) || CAST(X'09' AS TEXT) || (-1 * ST_Y(ST_Centroid(geom))) || CAST(X'09' AS TEXT) || AsSVG(geom, 1) || CAST(X'09' AS TEXT) || GeometryType(geom) || CAST(X'09' AS TEXT) || REPLACE(name,'&','and') FROM ${layer} WHERE geom NOT LIKE '%null%'" natural_earth_vector.gpkg | grep -e '=' | sed -e 's/^.*://g' -e 's/^.* = //g' | while IFS=$'\t' read -a array; do
    case ${array[4]} in
      POINT|MULTIPOINT)
        echo '<circle id="'${array[0]}'" cx="'${array[1]}'" cy="'${array[2]}'" r="1em" vector-effect="non-scaling-stroke" fill="#FFF" fill-opacity="1" stroke="#000" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round"><title>'${array[5]}'</title></circle>' >> svg/${layer}.svg
        ;;
      LINESTRING|MULTILINESTRING)
        echo '<path id="'${array[0]}'" d="'${array[3]}'" vector-effect="non-scaling-stroke" stroke="#000" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round"><title>'${array[5]}'</title></path>' >> svg/${layer}.svg
        ;;
      POLYGON|MULTIPOLYGON)
        echo '<path id="'${array[0]}'" d="'${array[3]}'" vector-effect="non-scaling-stroke" fill="#000" fill-opacity="1" stroke="#FFF" stroke-width="0.6px" stroke-linejoin="round" stroke-linecap="round"><title>'${array[5]}'</title></path>' >> svg/${layer}.svg
        ;;
    esac
  done
  echo '</svg>' >> svg/${layer}.svg
done
```
