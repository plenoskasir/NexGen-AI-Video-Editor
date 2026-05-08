<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NexGen AI Video Editor</title>
    
    <!-- 1. Tailwind CSS (Styling) -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- 2. FontAwesome (Untuk Icon) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- 3. React & ReactDOM -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    
    <!-- 4. Babel (Agar bisa baca kode React di HTML) -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <style>
        /* CSS Tambahan untuk Scrollbar & Animasi */
        body { margin: 0; background-color: #020617; color: white; overflow-x: hidden; }
        .custom-scrollbar::-webkit-scrollbar { width: 6px; height: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: rgba(0,0,0,0.2); }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.1); border-radius: 10px; }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover { background: rgba(255,255,255,0.2); }
        @keyframes scanline {
            0% { transform: translateY(-100%); }
            100% { transform: translateY(100%); }
        }
        .animate-scanline { animation: scanline 3s linear infinite; }
    </style>
</head>
<body>
    <!-- Tempat aplikasi React akan muncul -->
    <div id="root"></div>

    <!-- KODE REACT UTAMA -->
    <script type="text/babel">
        const { useState, useEffect } = React;

        // --- MOCK DATA ---
        const aiScenes = [
            { id: 1, start: '00:00', end: '00:15', type: 'Strong Hook', emotion: 'High (Excited)', score: 98, color: 'text-purple-400 bg-purple-500/20 border-purple-500/50' },
            { id: 2, start: '00:15', end: '00:32', type: 'Story Build', emotion: 'Medium', score: 85, color: 'text-blue-400 bg-blue-500/20 border-blue-500/50' },
            { id: 3, start: '00:32', end: '00:45', type: 'Punchline/Viral', emotion: 'Peak', score: 99, color: 'text-pink-400 bg-pink-500/20 border-pink-500/50' },
            { id: 4, start: '00:45', end: '00:60', type: 'CTA', emotion: 'Neutral', score: 75, color: 'text-indigo-400 bg-indigo-500/20 border-indigo-500/50' },
        ];

        const processingSteps = [
            { id: 1, label: "Fetching Video Stream...", icon: "fas fa-cloud" },
            { id: 2, label: "Neural Audio Extraction", icon: "fas fa-music" },
            { id: 3, label: "Whisper AI Transcription", icon: "fas fa-font" },
            { id: 4, label: "Analyzing Viral Moments", icon: "fas fa-fire" },
            { id: 5, label: "Applying Smart Cuts & B-Roll", icon: "fas fa-cut" },
            { id: 6, label: "Generating Dynamic Subtitles", icon: "fas fa-bolt" },
        ];

        function App() {
            const [view, setView] = useState('landing');
            const [urlInput, setUrlInput] = useState('');
            const [processingState, setProcessingState] = useState(0);
            const [activeTab, setActiveTab] = useState('ai_analysis');
            const [isPlaying, setIsPlaying] = useState(false);
            const [currentTime, setCurrentTime] = useState(0);

            // Simulasi Timeline
            useEffect(() => {
                let interval;
                if (isPlaying) {
                    interval = setInterval(() => {
                        setCurrentTime((prev) => (prev >= 60 ? 0 : prev + 1));
                    }, 100);
                }
                return () => clearInterval(interval);
            }, [isPlaying]);

            const handleStartProcessing = (e) => {
                e.preventDefault();
                if (!urlInput) return;
                setView('processing');
                setProcessingState(0);
                
                let step = 0;
                const processInterval = setInterval(() => {
                    step++;
                    setProcessingState(step);
                    if (step >= processingSteps.length) {
                        clearInterval(processInterval);
                        setTimeout(() => setView('editor'), 800);
                    }
                }, 1500);
            };

            // --- LAYAR 1: LANDING PAGE ---
            if (view === 'landing') {
                return (
                    <div className="min-h-screen bg-gradient-to-br from-slate-950 via-[#0a0514] to-slate-950 flex flex-col items-center justify-center p-6 relative overflow-hidden text-white">
                        <div className="absolute top-1/4 left-1/4 w-96 h-96 bg-purple-600/20 rounded-full blur-[120px] pointer-events-none"></div>
                        <div className="absolute bottom-1/4 right-1/4 w-96 h-96 bg-blue-600/20 rounded-full blur-[120px] pointer-events-none"></div>

                        <div className="z-10 max-w-4xl w-full text-center space-y-8">
                            <div className="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-white/5 border border-white/10 mb-4 backdrop-blur-md">
                                <i className="fas fa-bolt text-purple-400"></i>
                                <span className="text-sm text-slate-300 tracking-wider">NEXGEN AI CORE v4.0 ONLINE</span>
                            </div>
                            
                            <h1 className="text-5xl md:text-7xl font-extrabold tracking-tight bg-clip-text text-transparent bg-gradient-to-r from-white via-purple-200 to-blue-200">
                                Turn Long Videos Into <br/> 
                                <span className="text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-blue-500">Viral Shorts Automatically.</span>
                            </h1>
                            
                            <p className="text-lg md:text-xl text-slate-400 max-w-2xl mx-auto">
                                Upload or paste a link. Our proprietary 2026 AI analyzes hooks, cuts silence, adds dynamic B-roll, and renders MrBeast-style captions in seconds.
                            </p>

                            <form onSubmit={handleStartProcessing} className="max-w-2xl mx-auto mt-12 relative group">
                                <div className="absolute -inset-1 bg-gradient-to-r from-purple-600 to-blue-600 rounded-2xl blur opacity-25 group-hover:opacity-75 transition duration-1000 group-hover:duration-200"></div>
                                <div className="relative flex items-center bg-slate-900 border border-white/10 rounded-2xl p-2 bg-slate-900/60 backdrop-blur-xl">
                                    <i className="fas fa-link text-slate-400 ml-4 text-xl"></i>
                                    <input 
                                        type="text" 
                                        placeholder="Paste YouTube, TikTok, or Drive link here..."
                                        className="flex-1 bg-transparent border-none text-white px-4 py-4 focus:outline-none placeholder-slate-500"
                                        value={urlInput}
                                        onChange={(e) => setUrlInput(e.target.value)}
                                        required
                                    />
                                    <button type="submit" className="px-8 py-4 bg-white text-black font-bold rounded-xl hover:bg-slate-200 transition-colors flex items-center gap-2 shadow-[0_0_20px_rgba(168,85,247,0.4)]">
                                        Generate <i className="fas fa-magic"></i>
                                    </button>
                                </div>
                            </form>

                            <div className="flex justify-center gap-8 mt-12 text-slate-500">
                                <div className="flex items-center gap-2"><i className="fas fa-upload"></i> Manual Upload</div>
                                <div className="flex items-center gap-2"><i className="fas fa-cloud"></i> Google Drive</div>
                                <div className="flex items-center gap-2"><i className="fas fa-server"></i> Local Rendering</div>
                            </div>
                        </div>
                    </div>
                );
            }

            // --- LAYAR 2: LOADING PROCESSING ---
            if (view === 'processing') {
                return (
                    <div className="min-h-screen bg-gradient-to-br from-slate-950 via-[#0a0514] to-slate-950 flex flex-col items-center justify-center p-6 text-white">
                        <div className="max-w-md w-full p-8 rounded-3xl bg-slate-900/60 backdrop-blur-xl border border-purple-500/30 shadow-[0_0_20px_rgba(168,85,247,0.4)] relative overflow-hidden">
                            <div className="absolute inset-0 bg-gradient-to-b from-transparent via-purple-500/10 to-transparent w-full h-full animate-scanline pointer-events-none"></div>

                            <div className="text-center mb-8">
                                <div className="inline-block p-4 bg-purple-500/10 rounded-full mb-4">
                                    <i className="fas fa-microchip text-4xl text-purple-400 animate-pulse"></i>
                                </div>
                                <h2 className="text-2xl font-bold">Neural Engine Processing</h2>
                                <p className="text-slate-400 text-sm mt-2">Allocating cloud GPUs for video analysis...</p>
                            </div>

                            <div className="space-y-4">
                                {processingSteps.map((step, index) => {
                                    const isActive = index === processingState;
                                    const isDone = index < processingState;
                                    
                                    return (
                                        <div key={step.id} className={`flex items-center gap-4 p-3 rounded-xl transition-all duration-300 ${isActive ? 'bg-white/10 border border-white/20' : 'opacity-50'}`}>
                                            <div className={`p-2 rounded-lg ${isDone ? 'bg-green-500/20 text-green-400' : isActive ? 'bg-purple-500/20 text-purple-400' : 'bg-slate-800 text-slate-500'}`}>
                                                {isDone ? <i className="fas fa-check-circle"></i> : isActive ? <i className="fas fa-spinner fa-spin"></i> : <i className={step.icon}></i>}
                                            </div>
                                            <span className={`flex-1 text-sm ${isActive ? 'text-white font-medium' : 'text-slate-400'}`}>
                                                {step.label}
                                            </span>
                                            {isActive && <span className="text-xs text-purple-400 animate-pulse">Running</span>}
                                        </div>
                                    );
                                })}
                            </div>

                            <div className="mt-8 pt-6 border-t border-white/10 flex justify-between items-center text-xs text-slate-500">
                                <span className="flex items-center gap-1"><i className="fas fa-hdd"></i> RAM: 32GB Allocated</span>
                                <span className="flex items-center gap-1"><i className="fas fa-bolt text-yellow-500"></i> Est. 12s remaining</span>
                            </div>
                        </div>
                    </div>
                );
            }

            // --- LAYAR 3: DASHBOARD EDITOR ---
            return (
                <div className="h-screen flex flex-col bg-gradient-to-br from-slate-950 via-[#0a0514] to-slate-950 text-white overflow-hidden">
                    {/* Header */}
                    <header className="h-16 bg-slate-900/60 backdrop-blur-xl border-b border-white/10 flex items-center justify-between px-6 z-20">
                        <div className="flex items-center gap-4">
                            <div className="flex items-center gap-2 text-xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-purple-400 to-blue-500">
                                <i className="fas fa-columns text-purple-500"></i> NexGen
                            </div>
                            <div className="h-6 w-px bg-white/10 mx-2"></div>
                            <span className="text-sm font-medium">Project_Viral_001.mp4</span>
                            <span className="px-2 py-1 rounded bg-green-500/10 text-green-400 text-xs flex items-center gap-1">
                                <i className="fas fa-cloud"></i> Saved
                            </span>
                        </div>
                        <div className="flex items-center gap-4">
                            <div className="flex items-center gap-2 text-xs text-slate-400 bg-slate-900 px-3 py-1.5 rounded-full border border-white/5">
                                <i className="fas fa-chart-line text-blue-400"></i> CPU: 12%
                            </div>
                            <button className="p-2 text-slate-400 hover:text-white transition-colors"><i className="fas fa-cog text-lg"></i></button>
                            <button className="px-4 py-2 bg-gradient-to-r from-purple-600 to-blue-600 rounded-lg font-medium text-sm flex items-center gap-2 hover:opacity-90 shadow-[0_0_20px_rgba(168,85,247,0.4)]">
                                <i className="fas fa-download"></i> Export
                            </button>
                        </div>
                    </header>

                    {/* Workspace */}
                    <div className="flex-1 flex overflow-hidden">
                        {/* Toolbar Kiri */}
                        <div className="w-16 border-r border-white/10 bg-slate-900/60 backdrop-blur-xl flex flex-col items-center py-6 gap-6 z-10">
                            <button className="p-3 bg-purple-500/20 text-purple-400 rounded-xl hover:bg-purple-500/40 transition"><i className="fas fa-magic text-xl"></i></button>
                            <button className="p-3 text-slate-400 hover:text-white hover:bg-white/5 rounded-xl transition"><i className="fas fa-video text-xl"></i></button>
                            <button className="p-3 text-slate-400 hover:text-white hover:bg-white/5 rounded-xl transition"><i className="fas fa-font text-xl"></i></button>
                            <button className="p-3 text-slate-400 hover:text-white hover:bg-white/5 rounded-xl transition"><i className="fas fa-layer-group text-xl"></i></button>
                        </div>

                        {/* Center: Video Preview */}
                        <div className="flex-1 flex flex-col lg:flex-row h-full">
                            <div className="flex-1 flex flex-col p-4">
                                <div className="flex-1 rounded-2xl bg-black border border-white/10 relative flex items-center justify-center overflow-hidden shadow-2xl">
                                    <div className="absolute inset-0 bg-gradient-to-b from-slate-900 to-black"></div>
                                    <div className="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 w-full text-center pointer-events-none z-10 px-12">
                                        <h2 className="text-4xl md:text-5xl font-black uppercase text-white tracking-tight drop-shadow-[0_4px_4px_rgba(0,0,0,0.8)]" style={{ WebkitTextStroke: '1px black' }}>
                                            <span className="text-yellow-400">THIS</span> IS THE FUTURE <br/>OF <span className="text-purple-400 bg-white/10 px-2 rounded backdrop-blur-sm">VIDEO EDITING</span>
                                        </h2>
                                    </div>
                                    <div className="absolute top-4 left-4 bg-blue-500/20 text-blue-400 px-3 py-1 rounded border border-blue-500/50 text-xs flex items-center gap-1">
                                        <i className="fas fa-video"></i> AI B-Roll Applied
                                    </div>
                                    <div className="absolute bottom-4 left-1/2 transform -translate-x-1/2 flex items-center gap-4 bg-slate-900/80 backdrop-blur-md px-6 py-3 rounded-full border border-white/10">
                                        <button onClick={() => setIsPlaying(!isPlaying)} className="hover:text-purple-400 transition text-lg w-6">
                                            {isPlaying ? <i className="fas fa-pause"></i> : <i className="fas fa-play"></i>}
                                        </button>
                                        <div className="w-48 h-1 bg-slate-700 rounded-full relative">
                                            <div className="absolute top-0 left-0 h-full bg-purple-500 rounded-full transition-all" style={{ width: `${(currentTime/60)*100}%` }}></div>
                                        </div>
                                        <span className="text-xs font-mono">00:{(currentTime/10).toFixed(1).replace('.','')} / 00:60</span>
                                    </div>
                                </div>
                            </div>

                            {/* Panel Kanan */}
                            <div className="w-full lg:w-[400px] border-l border-white/10 bg-slate-900/60 backdrop-blur-xl flex flex-col z-10">
                                <div className="flex border-b border-white/10">
                                    <button onClick={() => setActiveTab('ai_analysis')} className={`flex-1 py-4 text-sm font-medium border-b-2 transition-colors ${activeTab === 'ai_analysis' ? 'border-purple-500 text-purple-400' : 'border-transparent text-slate-400'}`}>AI Analysis</button>
                                    <button onClick={() => setActiveTab('subtitles')} className={`flex-1 py-4 text-sm font-medium border-b-2 transition-colors ${activeTab === 'subtitles' ? 'border-purple-500 text-purple-400' : 'border-transparent text-slate-400'}`}>Subtitles</button>
                                    <button onClick={() => setActiveTab('duration')} className={`flex-1 py-4 text-sm font-medium border-b-2 transition-colors ${activeTab === 'duration' ? 'border-purple-500 text-purple-400' : 'border-transparent text-slate-400'}`}>Format</button>
                                </div>

                                <div className="flex-1 overflow-y-auto p-6 space-y-6 custom-scrollbar">
                                    {/* TABS CONTENT */}
                                    {activeTab === 'ai_analysis' && (
                                        <>
                                            <div className="p-5 rounded-2xl bg-gradient-to-br from-purple-900/40 to-slate-900 border border-purple-500/30 shadow-[0_0_20px_rgba(168,85,247,0.4)]">
                                                <div className="flex justify-between items-center mb-4">
                                                    <h3 className="font-bold flex items-center gap-2"><i className="fas fa-fire text-orange-500"></i> Viral Potential</h3>
                                                    <span className="text-2xl font-black text-white">96<span className="text-sm text-slate-400">%</span></span>
                                                </div>
                                                <div className="space-y-2 text-sm text-slate-300">
                                                    <div className="flex justify-between items-center"><span>Hook Strength</span><div className="w-24 h-2 bg-slate-800 rounded-full"><div className="h-full bg-green-500 w-[98%] rounded-full"></div></div></div>
                                                    <div className="flex justify-between items-center"><span>Retention Prediction</span><div className="w-24 h-2 bg-slate-800 rounded-full"><div className="h-full bg-blue-500 w-[85%] rounded-full"></div></div></div>
                                                    <div className="flex justify-between items-center"><span>Pacing Rating</span><div className="w-24 h-2 bg-slate-800 rounded-full"><div className="h-full bg-purple-500 w-[92%] rounded-full"></div></div></div>
                                                </div>
                                            </div>
                                            <div>
                                                <h3 className="text-sm font-semibold text-slate-400 mb-3 uppercase tracking-wider">Smart Scene Breakdown</h3>
                                                <div className="space-y-3">
                                                    {aiScenes.map((scene) => (
                                                        <div key={scene.id} className="p-3 rounded-xl bg-slate-800/50 border border-white/5 cursor-pointer flex flex-col gap-2">
                                                            <div className="flex justify-between items-center">
                                                                <span className="text-xs font-mono text-slate-400">{scene.start} - {scene.end}</span>
                                                                <span className={`text-[10px] uppercase px-2 py-0.5 rounded-full font-bold border ${scene.color}`}>{scene.type}</span>
                                                            </div>
                                                            <div className="flex justify-between text-sm">
                                                                <span>Emotion: <span className="text-white">{scene.emotion}</span></span>
                                                                <span className="text-green-400">Score: {scene.score}</span>
                                                            </div>
                                                        </div>
                                                    ))}
                                                </div>
                                            </div>
                                        </>
                                    )}

                                    {activeTab === 'subtitles' && (
                                        <>
                                            <div>
                                                <h3 className="text-sm font-semibold text-slate-400 mb-3 uppercase tracking-wider">Auto Styling Preset</h3>
                                                <div className="grid grid-cols-2 gap-3">
                                                    {['MrBeast', 'Hormozi', 'Neon Glow', 'Cinematic', 'Gaming', 'Minimal'].map((style, i) => (
                                                        <button key={style} className={`p-3 text-sm rounded-xl border ${i === 0 ? 'bg-purple-600/20 border-purple-500 text-white' : 'bg-slate-800/50 border-white/5 text-slate-400'}`}>{style}</button>
                                                    ))}
                                                </div>
                                            </div>
                                            <div className="space-y-4 pt-4 border-t border-white/10">
                                                <div className="flex justify-between items-center"><span className="text-sm">Auto Emoji Insert</span><div className="w-10 h-5 bg-purple-600 rounded-full relative"><div className="absolute right-1 top-1 w-3 h-3 bg-white rounded-full"></div></div></div>
                                                <div className="flex justify-between items-center"><span className="text-sm">Highlight Keywords</span><div className="w-10 h-5 bg-purple-600 rounded-full relative"><div className="absolute right-1 top-1 w-3 h-3 bg-white rounded-full"></div></div></div>
                                                <div className="flex justify-between items-center"><span className="text-sm">Dynamic Animations</span><div className="w-10 h-5 bg-purple-600 rounded-full relative"><div className="absolute right-1 top-1 w-3 h-3 bg-white rounded-full"></div></div></div>
                                            </div>
                                        </>
                                    )}

                                    {activeTab === 'duration' && (
                                        <>
                                            <div className="p-4 rounded-xl bg-blue-900/20 border border-blue-500/30 flex items-start gap-3">
                                                <i className="fas fa-bolt text-blue-400 mt-1"></i>
                                                <div>
                                                    <h4 className="font-semibold text-blue-100 text-sm">AI Smart Duration</h4>
                                                    <p className="text-xs text-blue-300/70 mt-1">AI analyzes narrative arc to suggest the highest retention length.</p>
                                                </div>
                                            </div>
                                            <div>
                                                <h3 className="text-sm font-semibold text-slate-400 mb-3 uppercase tracking-wider mt-6">Select Duration</h3>
                                                <div className="space-y-2">
                                                    <div className="flex justify-between items-center p-3 bg-purple-900/20 border border-purple-500/50 rounded-xl cursor-pointer">
                                                        <span className="font-medium text-white">45 Seconds</span>
                                                        <span className="text-xs text-purple-400 font-bold bg-purple-500/20 px-2 py-1 rounded">Recommended: 92% Viral</span>
                                                    </div>
                                                    <div className="flex justify-between items-center p-3 bg-slate-800/50 border border-white/5 rounded-xl cursor-pointer">
                                                        <span className="font-medium text-slate-300">60 Seconds</span>
                                                        <span className="text-xs text-slate-500">Best Retention</span>
                                                    </div>
                                                </div>
                                            </div>
                                        </>
                                    )}
                                </div>
                            </div>
                        </div>
                    </div>

                    {/* Timeline */}
                    <div className="h-[300px] border-t border-white/10 bg-slate-900/60 backdrop-blur-xl flex flex-col z-20">
                        <div className="h-10 border-b border-white/5 flex items-center px-4 gap-4 text-slate-400 text-sm">
                            <button className="hover:text-white"><i className="fas fa-cut"></i></button>
                            <div className="w-px h-4 bg-white/10"></div>
                            <button className="hover:text-white">Auto Cut Silence</button>
                            <button className="hover:text-white">AI Reframing</button>
                        </div>
                        <div className="flex-1 overflow-x-auto overflow-y-hidden relative p-4 custom-scrollbar">
                            <div className="relative mt-2 space-y-2 min-w-[800px]">
                                {/* Playhead */}
                                <div className="absolute top-0 bottom-0 w-px bg-red-500 z-50 pointer-events-none" style={{ left: `${(currentTime/60)*100}%` }}>
                                    <div className="absolute -top-3 -left-1.5 w-0 h-0 border-l-[6px] border-r-[6px] border-t-[8px] border-transparent border-t-red-500"></div>
                                </div>

                                {/* Video Track */}
                                <div className="flex gap-2">
                                    <div className="w-20 text-xs text-slate-500 shrink-0 py-2">V1 Main</div>
                                    <div className="flex-1 h-12 bg-slate-800 rounded-md border border-white/5 flex overflow-hidden">
                                        {[1,2,3,4,5,6].map(i => (
                                            <div key={i} className="h-full w-24 border-r border-slate-700/50 bg-slate-900 flex items-center justify-center">
                                                <i className="fas fa-video text-slate-700 text-xs"></i>
                                            </div>
                                        ))}
                                    </div>
                                </div>

                                {/* Audio Track */}
                                <div className="flex gap-2">
                                    <div className="w-20 text-xs text-slate-500 shrink-0 py-2">A1 Audio</div>
                                    <div className="flex-1 h-12 bg-slate-800/50 rounded-md border border-white/5 overflow-hidden flex items-center px-1">
                                        {[...Array(100)].map((_, i) => {
                                            if(i > 20 && i < 25) return <div key={i} className="w-[1%] h-0 mr-[1px]"></div>;
                                            const height = Math.random() * 80 + 20;
                                            return <div key={i} className={`flex-1 mr-[1px] rounded-full ${i > 30 && i < 40 ? 'bg-orange-500' : 'bg-emerald-500/60'}`} style={{ height: `${height}%` }}></div>
                                        })}
                                    </div>
                                </div>

                                {/* Subtitle Track */}
                                <div className="flex gap-2 mt-4">
                                    <div className="w-20 text-xs text-slate-500 shrink-0 py-1">S1 Captions</div>
                                    <div className="flex-1 h-8 bg-slate-800/30 rounded-md border border-white/5 flex gap-1 p-1">
                                        <div className="w-[15%] h-full bg-yellow-500/20 border border-yellow-500/50 text-[10px] text-yellow-200 flex items-center justify-center rounded">"THIS IS"</div>
                                        <div className="w-[20%] h-full bg-yellow-500/20 border border-yellow-500/50 text-[10px] text-yellow-200 flex items-center justify-center rounded">"THE FUTURE"</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            );
        }

        // Render aplikasi ke dalam div#root
        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
