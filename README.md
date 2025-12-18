"use client"

import { Canvas } from "@react-three/fiber"
import { OrbitControls, Environment, PerspectiveCamera, Float } from "@react-three/drei"
import { Suspense, useState } from "react"
import { Button } from "@/components/ui/button"
import { Card } from "@/components/ui/card"
import { Clock, Award, Zap } from "lucide-react"

function Watch3D({ position = [0, 0, 0] }: { position?: [number, number, number] }) {
  return (
    <Float speed={1.5} rotationIntensity={0.5} floatIntensity={0.5}>
      <group position={position}>
        {/* Watch case */}
        <mesh castShadow>
          <cylinderGeometry args={[1.5, 1.5, 0.5, 64]} />
          <meshStandardMaterial color="#1a1a1a" metalness={0.9} roughness={0.1} />
        </mesh>

        {/* Watch bezel */}
        <mesh position={[0, 0.26, 0]} castShadow>
          <cylinderGeometry args={[1.52, 1.52, 0.02, 64]} />
          <meshStandardMaterial color="#d4af37" metalness={1} roughness={0.2} />
        </mesh>

        {/* Watch face */}
        <mesh position={[0, 0.251, 0]} rotation={[-Math.PI / 2, 0, 0]}>
          <circleGeometry args={[1.4, 64]} />
          <meshStandardMaterial color="#0a0a0a" metalness={0.3} roughness={0.7} />
        </mesh>

        {/* Hour markers */}
        {[...Array(12)].map((_, i) => {
          const angle = (i * Math.PI) / 6
          const x = Math.cos(angle) * 1.1
          const z = Math.sin(angle) * 1.1
          return (
            <mesh key={i} position={[x, 0.27, z]}>
              <boxGeometry args={[0.05, 0.01, 0.15]} />
              <meshStandardMaterial color="#d4af37" metalness={1} roughness={0.2} />
            </mesh>
          )
        })}

        {/* Hour hand */}
        <mesh position={[0, 0.28, -0.3]} rotation={[0, 0, Math.PI / 6]}>
          <boxGeometry args={[0.08, 0.01, 0.6]} />
          <meshStandardMaterial color="#d4af37" metalness={1} roughness={0.2} />
        </mesh>

        {/* Minute hand */}
        <mesh position={[0, 0.29, -0.5]} rotation={[0, 0, -Math.PI / 4]}>
          <boxGeometry args={[0.06, 0.01, 1]} />
          <meshStandardMaterial color="#ffffff" metalness={0.8} roughness={0.3} />
        </mesh>

        {/* Second hand */}
        <mesh position={[0, 0.3, -0.4]} rotation={[0, 0, Math.PI / 3]}>
          <boxGeometry args={[0.03, 0.01, 0.9]} />
          <meshStandardMaterial color="#c41e3a" metalness={0.9} roughness={0.1} />
        </mesh>

        {/* Center dot */}
        <mesh position={[0, 0.31, 0]}>
          <cylinderGeometry args={[0.1, 0.1, 0.02, 32]} />
          <meshStandardMaterial color="#d4af37" metalness={1} roughness={0.1} />
        </mesh>

        {/* Crystal glass */}
        <mesh position={[0, 0.26, 0]}>
          <cylinderGeometry args={[1.45, 1.45, 0.1, 64]} />
          <meshPhysicalMaterial
            color="#ffffff"
            metalness={0}
            roughness={0}
            transmission={0.95}
            thickness={0.5}
            transparent
            opacity={0.3}
          />
        </mesh>

        {/* Watch strap left */}
        <mesh position={[-1.7, 0, 0]} castShadow>
          <boxGeometry args={[0.5, 0.3, 1.2]} />
          <meshStandardMaterial color="#1a1a1a" metalness={0.1} roughness={0.8} />
        </mesh>

        {/* Watch strap right */}
        <mesh position={[1.7, 0, 0]} castShadow>
          <boxGeometry args={[0.5, 0.3, 1.2]} />
          <meshStandardMaterial color="#1a1a1a" metalness={0.1} roughness={0.8} />
        </mesh>
      </group>
    </Float>
  )
}

function Scene() {
  return (
    <>
      <PerspectiveCamera makeDefault position={[0, 2, 8]} fov={50} />
      <OrbitControls
        enablePan={false}
        minDistance={5}
        maxDistance={12}
        minPolarAngle={Math.PI / 4}
        maxPolarAngle={Math.PI / 2}
      />

      <ambientLight intensity={0.5} />
      <spotLight position={[10, 10, 10]} angle={0.3} penumbra={1} intensity={2} castShadow />
      <spotLight position={[-10, 10, -10]} angle={0.3} penumbra={1} intensity={1.5} />

      <Suspense fallback={null}>
        <Watch3D />
        <Environment preset="studio" />
      </Suspense>

      {/* Ground plane for shadows */}
      <mesh rotation={[-Math.PI / 2, 0, 0]} position={[0, -2, 0]} receiveShadow>
        <planeGeometry args={[50, 50]} />
        <shadowMaterial opacity={0.2} />
      </mesh>
    </>
  )
}

const watches = [
  {
    name: "Cronógrafo Elite",
    price: "$3,499",
    description: "Precisión suiza con movimiento automático",
    image:
      "https://placehold.co/400x400?text=Luxury+chronograph+watch+with+black+leather+strap+golden+bezel+and+white+dial",
  },
  {
    name: "Classic Minimal",
    price: "$1,899",
    description: "Elegancia atemporal en cada detalle",
    image: "https://placehold.co/400x400?text=Minimalist+luxury+watch+with+silver+case+black+dial+and+leather+strap",
  },
  {
    name: "Sport Master",
    price: "$2,799",
    description: "Diseño robusto para aventureros",
    image: "https://placehold.co/400x400?text=Sport+luxury+watch+with+steel+bracelet+blue+dial+and+rotating+bezel",
  },
]

const features = [
  {
    icon: Clock,
    title: "Movimiento Suizo",
    description: "Mecanismos de precisión fabricados en Suiza con certificación cronómetro",
  },
  {
    icon: Award,
    title: "Garantía de por Vida",
    description: "Respaldo total en cada pieza. Calidad garantizada para generaciones",
  },
  {
    icon: Zap,
    title: "Diseño Innovador",
    description: "Fusión perfecta entre tradición artesanal y tecnología moderna",
  },
]

export default function Home() {
  const [selectedWatch, setSelectedWatch] = useState(0)

  return (
    <div className="min-h-screen bg-sand text-charcoal">
      {/* Header */}
      <header className="fixed top-0 left-0 right-0 z-50 bg-sand/80 backdrop-blur-md border-b border-charcoal/10">
        <div className="container mx-auto px-6 py-4 flex items-center justify-between">
          <div className="text-2xl font-serif font-bold tracking-tight text-charcoal">HOROLOGÍA</div>
          <nav className="hidden md:flex items-center gap-8">
            <a href="#coleccion" className="text-sm hover:text-gold transition-colors">
              Colección
            </a>
            <a href="#craftmanship" className="text-sm hover:text-gold transition-colors">
              Artesanía
            </a>
            <a href="#contacto" className="text-sm hover:text-gold transition-colors">
              Contacto
            </a>
          </nav>
          <Button
            variant="outline"
            className="border-charcoal text-charcoal hover:bg-charcoal hover:text-sand bg-transparent"
          >
            Reservar Cita
          </Button>
        </div>
      </header>

      {/* Hero Section */}
      <section className="pt-32 pb-20 px-6">
        <div className="container mx-auto">
          <div className="text-center mb-8">
            <h1 className="text-5xl md:text-7xl font-serif font-bold mb-6 text-balance leading-tight">
              El tiempo se vuelve arte
            </h1>
            <p className="text-lg md:text-xl text-charcoal/70 max-w-2xl mx-auto text-pretty">
              Descubre la excelencia en relojería de lujo. Cada pieza cuenta una historia de precisión y elegancia.
            </p>
          </div>

          {/* 3D Watch Viewer */}
          <div className="w-full h-[500px] md:h-[600px] rounded-2xl overflow-hidden bg-gradient-to-br from-charcoal/5 to-gold/10 shadow-2xl">
            <Canvas shadows>
              <Scene />
            </Canvas>
          </div>

          <p className="text-center text-sm text-charcoal/60 mt-6">
            Interactúa con el modelo 3D • Arrastra para rotar • Desplaza para hacer zoom
          </p>
        </div>
      </section>

      {/* Features */}
      <section className="py-20 bg-charcoal text-sand">
        <div className="container mx-auto px-6">
          <div className="grid md:grid-cols-3 gap-12">
            {features.map((feature, idx) => (
              <div key={idx} className="text-center">
                <div className="inline-flex items-center justify-center w-16 h-16 rounded-full bg-gold/20 mb-6">
                  <feature.icon className="w-8 h-8 text-gold" />
                </div>
                <h3 className="text-xl font-serif font-bold mb-3">{feature.title}</h3>
                <p className="text-sand/70 text-pretty">{feature.description}</p>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* Collection */}
      <section id="coleccion" className="py-20 px-6">
        <div className="container mx-auto">
          <div className="text-center mb-16">
            <h2 className="text-4xl md:text-5xl font-serif font-bold mb-4">Colección Exclusiva</h2>
            <p className="text-charcoal/70 text-lg">Piezas únicas diseñadas para los que aprecian la perfección</p>
          </div>

          <div className="grid md:grid-cols-3 gap-8">
            {watches.map((watch, idx) => (
              <Card
                key={idx}
                className="overflow-hidden border-charcoal/10 hover:shadow-2xl transition-all duration-300 hover:-translate-y-2 cursor-pointer bg-sand"
                onClick={() => setSelectedWatch(idx)}
              >
                <div className="aspect-square overflow-hidden bg-gradient-to-br from-charcoal/5 to-gold/5">
                  <img
                    src={watch.image || "/placeholder.svg"}
                    alt={watch.name}
                    className="w-full h-full object-cover hover:scale-110 transition-transform duration-500"
                  />
                </div>
                <div className="p-6">
                  <h3 className="text-2xl font-serif font-bold mb-2 text-charcoal">{watch.name}</h3>
                  <p className="text-charcoal/60 mb-4">{watch.description}</p>
                  <div className="flex items-center justify-between">
                    <span className="text-2xl font-bold text-gold">{watch.price}</span>
                    <Button className="bg-charcoal text-sand hover:bg-gold hover:text-charcoal">Ver Detalles</Button>
                  </div>
                </div>
              </Card>
            ))}
          </div>
        </div>
      </section>

      {/* Craftsmanship */}
      <section id="craftmanship" className="py-20 px-6 bg-gradient-to-br from-gold/5 to-charcoal/5">
        <div className="container mx-auto">
          <div className="grid md:grid-cols-2 gap-12 items-center">
            <div>
              <h2 className="text-4xl md:text-5xl font-serif font-bold mb-6 text-balance">
                Artesanía que trasciende el tiempo
              </h2>
              <p className="text-charcoal/70 text-lg mb-6 leading-relaxed text-pretty">
                Cada reloj es el resultado de cientos de horas de trabajo meticuloso. Nuestros maestros relojeros
                combinan técnicas centenarias con tecnología de vanguardia para crear piezas que durarán generaciones.
              </p>
              <ul className="space-y-4 mb-8">
                <li className="flex items-start gap-3">
                  <div className="w-6 h-6 rounded-full bg-gold/20 flex items-center justify-center flex-shrink-0 mt-0.5">
                    <div className="w-2 h-2 rounded-full bg-gold"></div>
                  </div>
                  <span className="text-charcoal/80">Más de 200 componentes ensamblados a mano</span>
                </li>
                <li className="flex items-start gap-3">
                  <div className="w-6 h-6 rounded-full bg-gold/20 flex items-center justify-center flex-shrink-0 mt-0.5">
                    <div className="w-2 h-2 rounded-full bg-gold"></div>
                  </div>
                  <span className="text-charcoal/80">Materiales premium: acero inoxidable 316L y zafiro</span>
                </li>
                <li className="flex items-start gap-3">
                  <div className="w-6 h-6 rounded-full bg-gold/20 flex items-center justify-center flex-shrink-0 mt-0.5">
                    <div className="w-2 h-2 rounded-full bg-gold"></div>
                  </div>
                  <span className="text-charcoal/80">Resistencia al agua hasta 100 metros</span>
                </li>
              </ul>
              <Button size="lg" className="bg-charcoal text-sand hover:bg-gold hover:text-charcoal">
                Descubre el Proceso
              </Button>
            </div>
            <div className="relative">
              <div className="aspect-[4/5] rounded-2xl overflow-hidden shadow-2xl">
                <img
                  src="https://placehold.co/600x750?text=Master+watchmaker+working+on+luxury+timepiece+with+precision+tools+and+magnifying+glass"
                  alt="Master watchmaker working on luxury timepiece with precision tools and magnifying glass"
                  className="w-full h-full object-cover"
                />
              </div>
              <div className="absolute -bottom-6 -left-6 bg-gold text-charcoal p-8 rounded-xl shadow-xl max-w-xs">
                <div className="text-4xl font-bold mb-2">50+</div>
                <div className="text-sm">Años de experiencia en relojería de lujo</div>
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* CTA Section */}
      <section className="py-20 px-6 bg-charcoal text-sand">
        <div className="container mx-auto text-center">
          <h2 className="text-4xl md:text-5xl font-serif font-bold mb-6 text-balance">Visita nuestro showroom</h2>
          <p className="text-sand/70 text-lg mb-8 max-w-2xl mx-auto text-pretty">
            Experimenta nuestras colecciones en persona. Agenda una cita privada con nuestros expertos en relojería.
          </p>
          <Button size="lg" className="bg-gold text-charcoal hover:bg-sand hover:text-charcoal text-lg px-8">
            Reservar Cita Privada
          </Button>
        </div>
      </section>

      {/* Footer */}
      <footer id="contacto" className="py-12 px-6 border-t border-charcoal/10">
        <div className="container mx-auto">
          <div className="grid md:grid-cols-4 gap-8 mb-8">
            <div>
              <div className="text-2xl font-serif font-bold mb-4 text-charcoal">HOROLOGÍA</div>
              <p className="text-sm text-charcoal/60">Excelencia en relojería desde 1970</p>
            </div>
            <div>
              <h4 className="font-bold mb-4 text-charcoal">Colección</h4>
              <ul className="space-y-2 text-sm text-charcoal/70">
                <li>
                  <a href="#" className="hover:text-gold transition-colors">
                    Clásicos
                  </a>
                </li>
                <li>
                  <a href="#" className="hover:text-gold transition-colors">
                    Deportivos
                  </a>
                </li>
                <li>
                  <a href="#" className="hover:text-gold transition-colors">
                    Edición Limitada
                  </a>
                </li>
              </ul>
            </div>
            <div>
              <h4 className="font-bold mb-4 text-charcoal">Servicios</h4>
              <ul className="space-y-2 text-sm text-charcoal/70">
                <li>
                  <a href="#" className="hover:text-gold transition-colors">
                    Mantenimiento
                  </a>
                </li>
                <li>
                  <a href="#" className="hover:text-gold transition-colors">
                    Personalización
                  </a>
                </li>
                <li>
                  <a href="#" className="hover:text-gold transition-colors">
                    Garantía
                  </a>
                </li>
              </ul>
            </div>
            <div>
              <h4 className="font-bold mb-4 text-charcoal">Contacto</h4>
              <ul className="space-y-2 text-sm text-charcoal/70">
                <li>info@horologia.com</li>
                <li>+34 900 123 456</li>
                <li>Madrid, España</li>
              </ul>
            </div>
          </div>
          <div className="pt-8 border-t border-charcoal/10 text-center text-sm text-charcoal/60">
            <p>© 2025 Horología. Todos los derechos reservados.</p>
          </div>
        </div>
      </footer>
    </div>
  )
}
