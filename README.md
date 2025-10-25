# Eclipse---Race-Simulator
A next-generation mobility simulation engine built with React, Three.js, and AI analytics. Simulates Formula E–style races with real-time motion, predictive strategy insights, and interactive 3D visualization.
import React, { useRef } from 'react';
import { Canvas, useFrame } from '@react-three/fiber';
import { OrbitControls, PerspectiveCamera } from '@react-three/drei';

function Car({ color, offset = 0, running = true }) {
    const ref = useRef();
    const localTime = useRef(0);
    useFrame((_, delta) => {
        if (running) localTime.current += delta;
        const t = (localTime.current * 0.5 + offset) % 1;
        const angle = t * Math.PI * 2;
        const radius = 8;
        const x = Math.cos(angle) * radius;
        const z = Math.sin(angle) * radius;
        if (ref.current) {
            ref.current.position.set(x, 0.15, z);
            ref.current.rotation.y = -angle + Math.PI / 2;
        }
    });

    return (
        <mesh ref={ref} castShadow>
            <boxGeometry args={[0.5, 0.25, 1]} />
            <meshStandardMaterial color={color} />
        </mesh>
    );
}

export default function RaceTrack3D({ running = true }) {
    return (
        <Canvas shadows style={{ width: '100%', height: 480, borderRadius: 12 }}>
            <color attach="background" args={['#0a0a0a']} />
            <ambientLight intensity={0.6} />
            <directionalLight position={[5, 10, 5]} intensity={1.2} castShadow />
            <PerspectiveCamera makeDefault position={[10, 6, 10]} fov={50} />
            <OrbitControls enablePan={false} enableZoom />

            <mesh rotation-x={-Math.PI / 2} receiveShadow>
                <circleGeometry args={[8.5, 64]} />
                <meshStandardMaterial color="#111" />
            </mesh>

            {[0, 0.25, 0.5, 0.75].map((o, i) => (
                <Car
                    key={i}
                    offset={o}
                    running={running}
                    color={['#ef4444', '#3b82f6', '#10b981', '#f59e0b'][i]}
                />
            ))}
        </Canvas>
    );
}
import React, { useState } from 'react';
import RaceTrack3D from './components/RaceTrack3D';
import './index.css';

export default function App() {
    const [running, setRunning] = useState(true);

    return (
        <div className="min-h-screen bg-gray-900 text-gray-100 p-6 flex flex-col gap-6">
            <header className="flex justify-between items-center">
                <h1 className="text-2xl font-bold">Eclipse — 3D Race Simulator</h1>
                <button
                    onClick={() => setRunning(!running)}
                    className={`px-4 py-2 rounded ${running ? 'bg-red-500' : 'bg-green-500'}`}
                >
                    {running ? 'Pause' : 'Start'}
                </button>
            </header>

            <main className="grid grid-cols-3 gap-6 flex-1">
                <section className="col-span-2 bg-gray-800 rounded-xl p-4 relative">
                    <RaceTrack3D running={running} />
                </section>

                <section className="col-span-1 bg-gray-800 p-4 rounded-xl">
                    <h2 className="font-semibold mb-2">Live Analytics</h2>
                    <ul className="text-sm space-y-1">
                        <li>🏎️ 4 Cars Active</li>
                        <li>⚡ Real-time Lap Simulation</li>
                        <li>🧠 AI Predictive Analytics (Coming Soon)</li>
                    </ul>
                </section>
            </main>
        </div>
    );
}
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';
import './index.css';

const root = createRoot(document.getElementById('root'));
root.render(<App />);
Rename-Item -Path ".\# Eclipse — AI-Powered Race Simulator.js" -NewName "Eclipse — AI-Powered Race Simulator.md"
npm install
npm install three @react-three/fiber @react-three/drei leva
npm start
