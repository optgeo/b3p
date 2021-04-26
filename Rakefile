require 'json'

URL = 'https://gsi-cyberjapan.github.io/gsivectortile-3d-like-building/building3dphoto.json'
FN = 'building3dphoto.json'
ST = 'docs/style.json'

desc 'download the original style file'
task :download do
  sh "curl -O #{URL}"
end

desc 'host the site'
task :host do
  sh "budo -d docs"
end

desc 'create style'
task :style do
  style = JSON.load(File.open(FN).read)
  style['sources']['h'] = {
    :maxzoom => 13,
    :minzoom => 3,
    :tileSize => 512,
    :tiles => [
      'https://optgeo.github.io/10b512-7-113-50/zxy/{z}/{x}/{y}.webp'
    ],
    :type => 'raster-dem'
  }
  style['terrain'] = {
    :source => 'h'
  }
  style['layers'].each {|layer|
    layer.delete('maxzoom') if layer['maxzoom'] == 18
    layer.delete('metadata')
    case layer['paint'] && layer['paint']['fill-extrusion-height']
    when 10
      layer['paint']['fill-extrusion-height'] = 4
    when 40
      layer['paint']['fill-extrusion-height'] = 15
    when 100
      layer['paint']['fill-extrusion-height'] = 40
    end
  }
  style['layers'].unshift({
    :id => 'sky',
    :paint => {
      'sky-type' => 'atmosphere'
    },
    :type => 'sky'
  })
  w = File.open(ST, 'w')
  w.print JSON.generate(style), "\n"
  w.close
end
