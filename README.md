<!DOCTYPE html>
<html lang="pt-BR" class="dark scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SmartTraffic AI - Gestão Inteligente de Trânsito</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Plus Jakarta Sans -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            dark: '#070A11',
                            card: '#0F172A',
                            border: '#1E293B',
                            emerald: '#10B981',
                            amber: '#F59E0B',
                            rose: '#EF4444',
                            cyan: '#06B6D4'
                        }
                    },
                    animation: {
                        'pulse-glow': 'pulseGlow 2s infinite',
                        'scan-line': 'scanLine 2.5s ease-in-out infinite alternate',
                        'car-flow': 'carFlow 4s linear infinite',
                    },
                    keyframes: {
                        pulseGlow: {
                            '0%, 100%': { opacity: '1', boxShadow: '0 0 15px currentColor' },
                            '50%': { opacity: '0.5', boxShadow: '0 0 5px currentColor' },
                        },
                        scanLine: {
                            '0%': { top: '0%' },
                            '100%': { top: '90%' },
                        },
                        carFlow: {
                            '0%': { transform: 'translateX(-100%)' },
                            '100%': { transform: 'translateX(400%)' },
                        }
                    }
                }
            }
        }
    </script>
    <style>
        /* CSS Customizado para Efeitos de Iluminação e LED */
        .led-red.active {
            background-color: #ef4444;
            box-shadow: 0 0 35px #ef4444, 0 0 15px #ef4444;
        }
        .led-yellow.active {
            background-color: #f59e0b;
            box-shadow: 0 0 35px #f59e0b, 0 0 15px #f59e0b;
        }
        .led-green.active {
            background-color: #10b981;
            box-shadow: 0 0 35px #10b981, 0 0 15px #10b981;
        }
        .glass-panel {
            background: rgba(15, 23, 42, 0.75);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #070a11;
        }
        ::-webkit-scrollbar-thumb {
            background: #1e293b;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #10b981;
        }
    </style>
</head>
<body class="bg-[#070A11] text-slate-100 font-sans min-h-screen flex flex-col selection:bg-emerald-500 selection:text-black">

    <!-- CONTÊINER DE TOAST NOTIFICAÇÃO -->
    <div id="toast-container" class="fixed bottom-6 right-6 z-50 flex flex-col gap-3 pointer-events-none"></div>

    <!-- CABEÇALHO / NAVBAR -->
    <header class="sticky top-0 z-40 glass-panel border-b border-slate-800/80">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
           
            <!-- Logo -->
            <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('home')">
                <div class="w-11 h-11 rounded-xl bg-gradient-to-tr from-emerald-500 via-teal-500 to-cyan-500 p-[2px] shadow-lg shadow-emerald-500/20">
                    <div class="w-full h-full bg-slate-950 rounded-[10px] flex items-center justify-center">
                        <i class="fa-solid fa-traffic-light text-emerald-400 text-xl"></i>
                    </div>
                </div>
                <div>
                    <span class="text-xl font-extrabold tracking-tight bg-gradient-to-r from-white via-slate-200 to-emerald-400 bg-clip-text text-transparent">
                        SmartTraffic <span class="text-emerald-400 font-black">AI</span>
                    </span>
                    <span class="block text-[10px] font-mono text-slate-400 uppercase tracking-widest">Sistemas Urbanos Autônomos</span>
                </div>
            </div>

            <!-- Navegação Desktop -->
            <nav class="hidden md:flex items-center gap-1 bg-slate-900/90 p-1.5 rounded-full border border-slate-800">
                <button id="nav-home" onclick="navigateTo('home')" class="nav-btn px-4 py-2 rounded-full text-sm font-medium transition-all text-emerald-400 bg-emerald-500/10 border border-emerald-500/30">
                    <i class="fa-solid fa-house mr-1.5"></i>Início
                </button>
                <button id="nav-cadastro" onclick="navigateTo('cadastro')" class="nav-btn px-4 py-2 rounded-full text-sm font-medium text-slate-300 hover:text-white hover:bg-slate-800/60 transition-all">
                    <i class="fa-solid fa-id-card-clip mr-1.5"></i>Cadastro Facial
                </button>
                <button id="nav-prioridades" onclick="navigateTo('prioridades')" class="nav-btn px-4 py-2 rounded-full text-sm font-medium text-slate-300 hover:text-white hover:bg-slate-800/60 transition-all">
                    <i class="fa-solid fa-sliders mr-1.5"></i>Prioridades
                </button>
                <button id="nav-contato" onclick="navigateTo('contato')" class="nav-btn px-4 py-2 rounded-full text-sm font-medium text-slate-300 hover:text-white hover:bg-slate-800/60 transition-all">
                    <i class="fa-solid fa-headset mr-1.5"></i>Contato
                </button>
                <button id="nav-feedback" onclick="navigateTo('feedback')" class="nav-btn px-4 py-2 rounded-full text-sm font-medium text-slate-300 hover:text-white hover:bg-slate-800/60 transition-all">
                    <i class="fa-solid fa-comments mr-1.5"></i>Feedback
                </button>
            </nav>

            <!-- Status do Sistema & Mobile Menu Button -->
            <div class="flex items-center gap-4">
                <div class="hidden lg:flex items-center gap-2.5 px-3.5 py-1.5 rounded-full bg-emerald-950/40 border border-emerald-500/30 text-xs font-mono text-emerald-400">
                    <span class="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-pulse"></span>
                    <span>IA Ativa • Latência: 12ms</span>
                </div>

                <button id="mobile-menu-btn" onclick="toggleMobileMenu()" class="md:hidden p-2 rounded-lg bg-slate-800 text-slate-300 hover:text-white focus:outline-none">
                    <i class="fa-solid fa-bars text-lg"></i>
                </button>
            </div>
        </div>

        <!-- Menu Mobile Drawer -->
        <div id="mobile-menu" class="hidden md:hidden bg-slate-900 border-b border-slate-800 px-4 pt-3 pb-6 space-y-2">
            <button onclick="navigateTo('home'); toggleMobileMenu();" class="w-full text-left px-4 py-3 rounded-xl text-sm font-medium text-slate-200 hover:bg-slate-800">
                <i class="fa-solid fa-house w-6 text-emerald-400"></i>Início
            </button>
            <button onclick="navigateTo('cadastro'); toggleMobileMenu();" class="w-full text-left px-4 py-3 rounded-xl text-sm font-medium text-slate-200 hover:bg-slate-800">
                <i class="fa-solid fa-id-card-clip w-6 text-cyan-400"></i>Cadastro Facial
            </button>
            <button onclick="navigateTo('prioridades'); toggleMobileMenu();" class="w-full text-left px-4 py-3 rounded-xl text-sm font-medium text-slate-200 hover:bg-slate-800">
                <i class="fa-solid fa-sliders w-6 text-amber-400"></i>Prioridades
            </button>
            <button onclick="navigateTo('contato'); toggleMobileMenu();" class="w-full text-left px-4 py-3 rounded-xl text-sm font-medium text-slate-200 hover:bg-slate-800">
                <i class="fa-solid fa-headset w-6 text-blue-400"></i>Contato
            </button>
            <button onclick="navigateTo('feedback'); toggleMobileMenu();" class="w-full text-left px-4 py-3 rounded-xl text-sm font-medium text-slate-200 hover:bg-slate-800">
                <i class="fa-solid fa-comments w-6 text-purple-400"></i>Feedback
            </button>
        </div>
    </header>

    <!-- CONTEÚDO PRINCIPAL (SPA CONTAINER) -->
    <main class="flex-grow">

        <!-- ================= PAGE 1: HOME / INÍCIO ================= -->
        <section id="view-home" class="app-view max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-16">
           
            <!-- Hero Section -->
            <div class="relative rounded-3xl p-8 sm:p-12 overflow-hidden bg-gradient-to-b from-slate-900 via-slate-900/90 to-[#070A11] border border-slate-800">
                <!-- Grid pattern background overlay -->
                <div class="absolute inset-0 bg-[linear-gradient(to_right,#1e293b15_1px,transparent_1px),linear-gradient(to_bottom,#1e293b15_1px,transparent_1px)] bg-[size:3rem_3rem] pointer-events-none"></div>
                <div class="absolute -top-24 -right-24 w-96 h-96 bg-emerald-500/10 rounded-full blur-3xl pointer-events-none"></div>
               
                <div class="relative z-10 max-w-3xl space-y-6">
                    <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-cyan-950/60 border border-cyan-500/30 text-cyan-400 text-xs font-semibold uppercase tracking-wider">
                        <i class="fa-solid fa-microchip"></i> Engenharia de Tráfego 4.0
                    </div>

                    <h1 class="text-4xl sm:text-6xl font-black tracking-tight text-white leading-none">
                        Gestão de Trânsito Autónoma com <span class="bg-gradient-to-r from-emerald-400 via-teal-300 to-cyan-400 bg-clip-text text-transparent">Inteligência Artificial</span>
                    </h1>

                    <p class="text-lg text-slate-300 leading-relaxed">
                        O SmartTraffic AI analisa o fluxo viário em tempo real por visão computacional, otimizando o tempo dos semáforos, priorizando veículos de emergência e reduzindo congestionamentos urbanos em até 40%.
                    </p>

                    <div class="flex flex-wrap items-center gap-4 pt-2">
                        <button onclick="scrollToSimulator()" class="px-6 py-3.5 rounded-xl font-bold text-slate-950 bg-gradient-to-r from-emerald-400 to-teal-400 hover:from-emerald-300 hover:to-teal-300 shadow-lg shadow-emerald-500/25 transition-all transform hover:-translate-y-0.5 flex items-center gap-2">
                            <i class="fa-solid fa-play text-xs"></i> Testar Simulador Vivo
                        </button>
                        <button onclick="navigateTo('cadastro')" class="px-6 py-3.5 rounded-xl font-semibold text-slate-200 bg-slate-800/80 hover:bg-slate-800 border border-slate-700 transition-all flex items-center gap-2">
                            <i class="fa-solid fa-face-smile text-emerald-400"></i> Cadastrar Veículo VIP
                        </button>
                    </div>
                </div>
            </div>

            <!-- SIMULADOR INTERATIVO DE SEMÁFORO INTELIGENTE -->
            <div id="simulator-section" class="scroll-mt-24 space-y-6">
                <div class="flex flex-col md:flex-row md:items-end justify-between gap-4">
                    <div>
                        <h2 class="text-2xl sm:text-3xl font-extrabold text-white flex items-center gap-3">
                            <i class="fa-solid fa-circle-dot text-emerald-400"></i> Simulador em Tempo Real
                        </h2>
                        <p class="text-slate-400 text-sm mt-1">Veja a inteligência artificial adaptando as fases luminosas de acordo com eventos urbanos.</p>
                    </div>

                    <!-- Indicadores Rápidos do Simulador -->
                    <div class="flex items-center gap-2 bg-slate-900 p-2 rounded-xl border border-slate-800 text-xs">
                        <span class="text-slate-400">Modo Atual:</span>
                        <span id="sim-mode-badge" class="px-2.5 py-1 rounded-md font-bold bg-emerald-500/20 text-emerald-400 border border-emerald-500/30">AUTOMÁTICO - IA DETECTANDO</span>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-stretch">
                   
                    <!-- Coluna 1: Estrutura Física do Semáforo LED -->
                    <div class="lg:col-span-4 bg-slate-900/90 border border-slate-800 rounded-3xl p-6 flex flex-col items-center justify-between shadow-xl relative overflow-hidden">
                        <div class="w-full flex justify-between items-center text-xs font-mono text-slate-400 mb-4 border-b border-slate-800 pb-3">
                            <span>CRUZAMENTO #042</span>
                            <span>AV. PAULISTA X REBOUÇAS</span>
                        </div>

                        <!-- Caixa Metallic do Semáforo -->
                        <div class="w-40 bg-slate-950 p-5 rounded-3xl border-4 border-slate-800 shadow-2xl flex flex-col items-center gap-5 relative">
                            <!-- Sombra de fundo de LED -->
                            <div class="w-2.5 h-full absolute -left-4 top-0 bg-slate-800/50 rounded-l"></div>
                           
                            <!-- Luz Vermelha -->
                            <div class="flex flex-col items-center gap-1">
                                <div id="led-red" class="led-red w-20 h-20 rounded-full bg-red-950/40 border-2 border-red-900/50 flex items-center justify-center transition-all duration-300">
                                    <i class="fa-solid fa-hand text-red-950 text-2xl" id="icon-red"></i>
                                </div>
                                <span class="text-[10px] font-mono font-bold text-red-500/80">PARE</span>
                            </div>

                            <!-- Luz Amarela -->
                            <div class="flex flex-col items-center gap-1">
                                <div id="led-yellow" class="led-yellow w-20 h-20 rounded-full bg-amber-950/40 border-2 border-amber-900/50 flex items-center justify-center transition-all duration-300">
                                    <i class="fa-solid fa-triangle-exclamation text-amber-950 text-2xl" id="icon-yellow"></i>
                                </div>
                                <span class="text-[10px] font-mono font-bold text-amber-500/80">ATENÇÃO</span>
                            </div>

                            <!-- Luz Verde -->
                            <div class="flex flex-col items-center gap-1">
                                <div id="led-green" class="led-green w-20 h-20 rounded-full bg-emerald-950/40 border-2 border-emerald-900/50 flex items-center justify-center transition-all duration-300 active">
                                    <i class="fa-solid fa-person-walking text-emerald-200 text-2xl" id="icon-green"></i>
                                </div>
                                <span class="text-[10px] font-mono font-bold text-emerald-500/80">SIGA</span>
                            </div>
                        </div>

                        <!-- Display de Cronômetro Regressivo -->
                        <div class="mt-6 w-full text-center bg-slate-950 py-3 rounded-2xl border border-slate-800">
                            <span class="text-xs text-slate-400 block font-mono">TEMPO ESTIMADO DE FASE</span>
                            <span id="sim-timer" class="text-4xl font-black font-mono text-emerald-400">12</span>
                            <span class="text-xs text-slate-500 font-mono">segundos</span>
                        </div>
                    </div>

                    <!-- Coluna 2: Radar de Visão Computacional & Painel de Eventos Simulado -->
                    <div class="lg:col-span-8 space-y-6 flex flex-col justify-between">
                       
                        <!-- Tela de Visão Computacional (Simulada) -->
                        <div class="bg-slate-950 border border-slate-800 rounded-3xl p-6 relative overflow-hidden min-h-[260px] flex flex-col justify-between">
                            <!-- Overlay de linhas cibernéticas -->
                            <div class="absolute inset-0 opacity-10 bg-[radial-gradient(#10b981_1px,transparent_1px)] [background-size:16px_16px]"></div>
                           
                            <div class="relative z-10 flex justify-between items-start">
                                <div class="flex items-center gap-2">
                                    <span class="w-3 h-3 rounded-full bg-emerald-500 animate-ping"></span>
                                    <span class="text-xs font-mono font-bold text-emerald-400 uppercase">Câmera IA #01 • Processamento Local em Tempo Real</span>
                                </div>
                                <span class="text-xs font-mono text-slate-500" id="live-fps">60 FPS | Res: 4K HDR</span>
                            </div>

                            <!-- Pista de Carros Animada Visualmente -->
                            <div class="relative z-10 my-6 bg-slate-900/90 h-24 rounded-2xl border border-slate-800 flex items-center px-4 overflow-hidden">
                                <!-- Linha pontilhada da via -->
                                <div class="absolute w-full border-b-2 border-dashed border-slate-700 left-0"></div>
                               
                                <!-- Simulação de Veículo Detectado -->
                                <div id="sim-vehicle-box" class="relative z-20 flex items-center gap-3 bg-emerald-950/80 border border-emerald-500/50 p-2.5 rounded-xl shadow-lg transition-all duration-500">
                                    <div class="w-10 h-10 rounded-lg bg-emerald-500/20 flex items-center justify-center text-emerald-400 text-lg">
                                        <i id="sim-vehicle-icon" class="fa-solid fa-car"></i>
                                    </div>
                                    <div>
                                        <div class="text-xs font-bold text-white flex items-center gap-2">
                                            <span id="sim-vehicle-title">Fluxo Normal</span>
                                            <span id="sim-confidence-badge" class="px-1.5 py-0.5 text-[10px] rounded bg-emerald-500/30 text-emerald-300 font-mono">99.4% Confianca</span>
                                        </div>
                                        <div class="text-[11px] font-mono text-slate-400" id="sim-vehicle-desc">
                                            Densidade: 14 veículos/min
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <!-- Terminal de Logs da IA -->
                            <div class="relative z-10 bg-slate-900/80 rounded-xl p-3 border border-slate-800 font-mono text-xs text-slate-300 space-y-1">
                                <div class="text-slate-500 flex justify-between border-b border-slate-800/80 pb-1 text-[11px]">
                                    <span>LOG DE DECISÕES DO ALGORITMO</span>
                                    <span id="log-timestamp">12:00:00</span>
                                </div>
                                <p id="sim-ai-log" class="text-emerald-400">
                                    > IA: Via principal limpa. Fase verde estendida para otimizar consumo de combustível.
                                </p>
                            </div>
                        </div>

                        <!-- Botões de Eventos de Simulação Interativos -->
                        <div class="bg-slate-900/90 border border-slate-800 rounded-3xl p-6 space-y-4">
                            <h3 class="text-sm font-bold text-slate-300 uppercase tracking-wider font-mono">Injetar Eventos Urbanos no Semáforo</h3>
                           
                            <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
                                <button onclick="triggerEmergencyEvent()" class="p-3.5 rounded-xl bg-red-950/40 hover:bg-red-900/50 border border-red-500/40 text-red-300 font-semibold text-xs flex items-center justify-center gap-2 transition-all transform active:scale-95">
                                    <i class="fa-solid fa-truck-medical text-red-400 text-base"></i>
                                    Simular Ambulância (Prioridade Total)
                                </button>
                               
                                <button onclick="triggerTrafficPeakEvent()" class="p-3.5 rounded-xl bg-amber-950/40 hover:bg-amber-900/50 border border-amber-500/40 text-amber-300 font-semibold text-xs flex items-center justify-center gap-2 transition-all transform active:scale-95">
                                    <i class="fa-solid fa-car-burst text-amber-400 text-base"></i>
                                    Simular Pico de Congestionamento
                                </button>

                                <button onclick="triggerPedestrianEvent()" class="p-3.5 rounded-xl bg-cyan-950/40 hover:bg-cyan-900/50 border border-cyan-500/40 text-cyan-300 font-semibold text-xs flex items-center justify-center gap-2 transition-all transform active:scale-95">
                                    <i class="fa-solid fa-person-walking-with-cane text-cyan-400 text-base"></i>
                                    Simular Pedestre PNE / Idoso
                                </button>
                            </div>
                        </div>

                    </div>
                </div>
            </div>

            <!-- CARDS DE MÉTRICAS RÁPIDAS -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                <div class="glass-panel p-6 rounded-3xl border border-slate-800 hover:border-emerald-500/40 transition-all group">
                    <div class="w-12 h-12 rounded-2xl bg-emerald-500/10 border border-emerald-500/20 flex items-center justify-center text-emerald-400 text-xl mb-4 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-stopwatch"></i>
                    </div>
                    <span class="text-3xl font-black text-white tracking-tight">-38%</span>
                    <h4 class="text-sm font-semibold text-slate-300 mt-1">Tempo Médio de Espera</h4>
                    <p class="text-xs text-slate-500 mt-2">Redução drástica nas filas em horários de pico urbano.</p>
                </div>

                <div class="glass-panel p-6 rounded-3xl border border-slate-800 hover:border-cyan-500/40 transition-all group">
                    <div class="w-12 h-12 rounded-2xl bg-cyan-500/10 border border-cyan-500/20 flex items-center justify-center text-cyan-400 text-xl mb-4 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-bolt"></i>
                    </div>
                    <span class="text-3xl font-black text-white tracking-tight">+45%</span>
                    <h4 class="text-sm font-semibold text-slate-300 mt-1">Fluidez de Tráfego</h4>
                    <p class="text-xs text-slate-500 mt-2">Aumento de vazão de veículos em artérias principais.</p>
                </div>

                <div class="glass-panel p-6 rounded-3xl border border-slate-800 hover:border-rose-500/40 transition-all group">
                    <div class="w-12 h-12 rounded-2xl bg-rose-500/10 border border-rose-500/20 flex items-center justify-center text-rose-400 text-xl mb-4 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-truck-fast"></i>
                    </div>
                    <span class="text-3xl font-black text-white tracking-tight">100%</span>
                    <h4 class="text-sm font-semibold text-slate-300 mt-1">Livre Passagem Socorro</h4>
                    <p class="text-xs text-slate-500 mt-2">Zero travamentos para ambulâncias e bombeiros.</p>
                </div>

                <div class="glass-panel p-6 rounded-3xl border border-slate-800 hover:border-amber-500/40 transition-all group">
                    <div class="w-12 h-12 rounded-2xl bg-amber-500/10 border border-amber-500/20 flex items-center justify-center text-amber-400 text-xl mb-4 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-face-smile"></i>
                    </div>
                    <span class="text-3xl font-black text-white tracking-tight">99.8%</span>
                    <h4 class="text-sm font-semibold text-slate-300 mt-1">Precisão Biométrica</h4>
                    <p class="text-xs text-slate-500 mt-2">Reconhecimento instantâneo de motoristas cadastrados.</p>
                </div>
            </div>

            <!-- CARDS DOS QUATRO BLOCOS DE FUNCIONALIDADES -->
            <div class="space-y-6">
                <h3 class="text-2xl font-extrabold text-white">Módulos do Sistema SmartTraffic</h3>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                   
                    <!-- Bloco 1: Cadastro Facial -->
                    <div onclick="navigateTo('cadastro')" class="glass-panel p-8 rounded-3xl border border-slate-800 hover:border-cyan-500/50 cursor-pointer transition-all hover:-translate-y-1 group">
                        <div class="flex justify-between items-start mb-6">
                            <div class="w-14 h-14 rounded-2xl bg-cyan-500/10 border border-cyan-500/30 flex items-center justify-center text-cyan-400 text-2xl group-hover:bg-cyan-500 group-hover:text-black transition-all">
                                <i class="fa-solid fa-id-card-clip"></i>
                            </div>
                            <span class="px-3 py-1 rounded-full bg-slate-800 text-slate-400 text-xs font-mono">Módulo #01</span>
                        </div>
                        <h4 class="text-xl font-bold text-white group-hover:text-cyan-400 transition-colors">Cadastro Facial de Motoristas</h4>
                        <p class="text-slate-400 text-sm mt-2 leading-relaxed">
                            Cadastre a biometria facial de condutores de ambulâncias, bombeiros e frotas públicas para liberação verde automática nas vias.
                        </p>
                        <div class="mt-6 flex items-center gap-2 text-cyan-400 font-semibold text-sm">
                            <span>Acessar Cadastro</span>
                            <i class="fa-solid fa-arrow-right text-xs group-hover:translate-x-1 transition-transform"></i>
                        </div>
                    </div>

                    <!-- Bloco 2: Gestão de Prioridades -->
                    <div onclick="navigateTo('prioridades')" class="glass-panel p-8 rounded-3xl border border-slate-800 hover:border-amber-500/50 cursor-pointer transition-all hover:-translate-y-1 group">
                        <div class="flex justify-between items-start mb-6">
                            <div class="w-14 h-14 rounded-2xl bg-amber-500/10 border border-amber-500/30 flex items-center justify-center text-amber-400 text-2xl group-hover:bg-amber-500 group-hover:text-black transition-all">
                                <i class="fa-solid fa-sliders"></i>
                            </div>
                            <span class="px-3 py-1 rounded-full bg-slate-800 text-slate-400 text-xs font-mono">Módulo #02</span>
                        </div>
                        <h4 class="text-xl font-bold text-white group-hover:text-amber-400 transition-colors">Painel de Prioridades Viárias</h4>
                        <p class="text-slate-400 text-sm mt-2 leading-relaxed">
                            Ajuste pesos de prioridade para ônibus BRT, transporte escolar, pedestres e veículos oficiais em um painel dinâmico.
                        </p>
                        <div class="mt-6 flex items-center gap-2 text-amber-400 font-semibold text-sm">
                            <span>Configurar Algoritmos</span>
                            <i class="fa-solid fa-arrow-right text-xs group-hover:translate-x-1 transition-transform"></i>
                        </div>
                    </div>

                    <!-- Bloco 3: Contato Suporte -->
                    <div onclick="navigateTo('contato')" class="glass-panel p-8 rounded-3xl border border-slate-800 hover:border-emerald-500/50 cursor-pointer transition-all hover:-translate-y-1 group">
                        <div class="flex justify-between items-start mb-6">
                            <div class="w-14 h-14 rounded-2xl bg-emerald-500/10 border border-emerald-500/30 flex items-center justify-center text-emerald-400 text-2xl group-hover:bg-emerald-500 group-hover:text-black transition-all">
                                <i class="fa-solid fa-headset"></i>
                            </div>
                            <span class="px-3 py-1 rounded-full bg-slate-800 text-slate-400 text-xs font-mono">Módulo #03</span>
                        </div>
                        <h4 class="text-xl font-bold text-white group-hover:text-emerald-400 transition-colors">Atendimento & Prefeituras</h4>
                        <p class="text-slate-400 text-sm mt-2 leading-relaxed">
                            Canal direto com engenheiros de tráfego, suporte de central 24/7 e atendimento especial a órgãos governamentais.
                        </p>
                        <div class="mt-6 flex items-center gap-2 text-emerald-400 font-semibold text-sm">
                            <span>Falar Conosco</span>
                            <i class="fa-solid fa-arrow-right text-xs group-hover:translate-x-1 transition-transform"></i>
                        </div>
                    </div>

                    <!-- Bloco 4: Feedback -->
                    <div onclick="navigateTo('feedback')" class="glass-panel p-8 rounded-3xl border border-slate-800 hover:border-purple-500/50 cursor-pointer transition-all hover:-translate-y-1 group">
                        <div class="flex justify-between items-start mb-6">
                            <div class="w-14 h-14 rounded-2xl bg-purple-500/10 border border-purple-500/30 flex items-center justify-center text-purple-400 text-2xl group-hover:bg-purple-500 group-hover:text-black transition-all">
                                <i class="fa-solid fa-comments"></i>
                            </div>
                            <span class="px-3 py-1 rounded-full bg-slate-800 text-slate-400 text-xs font-mono">Módulo #04</span>
                        </div>
                        <h4 class="text-xl font-bold text-white group-hover:text-purple-400 transition-colors">Feedback & Relatos da Comunidade</h4>
                        <p class="text-slate-400 text-sm mt-2 leading-relaxed">
                            Envie sugestões de gargalos no seu bairro, avalie a experiência do trânsito e acompanhe o status de resolução.
                        </p>
                        <div class="mt-6 flex items-center gap-2 text-purple-400 font-semibold text-sm">
                            <span>Avaliar Semáforos</span>
                            <i class="fa-solid fa-arrow-right text-xs group-hover:translate-x-1 transition-transform"></i>
                        </div>
                    </div>

                </div>
            </div>

        </section>

        <!-- ================= PAGE 2: CADASTRO FACIAL ================= -->
        <section id="view-cadastro" class="app-view hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-10">
           
            <div class="border-b border-slate-800 pb-6">
                <span class="text-xs font-mono text-cyan-400 uppercase tracking-widest font-semibold">Módulo de Visão Computacional</span>
                <h2 class="text-3xl font-extrabold text-white mt-1">Cadastro Facial de Condutores de Emergência</h2>
                <p class="text-slate-400 text-sm mt-1">Ao aproximar o veículo do semáforo inteligente, o motorista é identificado por biometria facial de alta precisão.</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
               
                <!-- Leitor / Webcam Simulado -->
                <div class="lg:col-span-5 bg-slate-900 border border-slate-800 rounded-3xl p-6 flex flex-col items-center justify-between shadow-2xl relative">
                    <div class="w-full flex justify-between items-center text-xs font-mono text-slate-400 mb-4">
                        <span>SCANNER BIOMÉTRICO FACIAL</span>
                        <span id="camera-status-badge" class="text-emerald-400">● PRONTO</span>
                    </div>

                    <!-- Container da Webcam / Foto -->
                    <div class="w-full h-72 bg-slate-950 rounded-2xl border-2 border-dashed border-slate-800 relative overflow-hidden flex flex-col items-center justify-center">
                        <!-- Imagem de exemplo ou overlay de camera -->
                        <div id="camera-overlay" class="absolute inset-0 flex flex-col items-center justify-center text-slate-600 space-y-3">
                            <i class="fa-solid fa-user-shield text-5xl"></i>
                            <span class="text-xs font-mono">Posicione o rosto no centro</span>
                        </div>

                        <!-- Render de foto capturada -->
                        <img id="captured-photo" class="hidden w-full h-full object-cover rounded-2xl" src="" alt="Rosto Capturado">

                        <!-- Retículos de escaneamento cibernético -->
                        <div id="scanner-reticle" class="hidden absolute inset-0 border-2 border-cyan-500/50 m-6 rounded-2xl pointer-events-none">
                            <div class="absolute top-0 left-0 w-6 h-6 border-t-4 border-l-4 border-cyan-400"></div>
                            <div class="absolute top-0 right-0 w-6 h-6 border-t-4 border-r-4 border-cyan-400"></div>
                            <div class="absolute bottom-0 left-0 w-6 h-6 border-b-4 border-l-4 border-cyan-400"></div>
                            <div class="absolute bottom-0 right-0 w-6 h-6 border-b-4 border-r-4 border-cyan-400"></div>
                           
                            <!-- Linha de escaneamento animada -->
                            <div class="w-full h-1 bg-gradient-to-r from-transparent via-cyan-400 to-transparent absolute animate-scan-line shadow-[0_0_15px_#06b6d4]"></div>
                        </div>
                    </div>

                    <!-- Botões de Ação de Captura -->
                    <div class="w-full grid grid-cols-2 gap-3 mt-4">
                        <button type="button" onclick="simularCapturaCamera()" class="py-3 px-4 rounded-xl bg-cyan-950/60 border border-cyan-500/40 text-cyan-300 font-semibold text-xs flex items-center justify-center gap-2 hover:bg-cyan-900/50 transition-all">
                            <i class="fa-solid fa-camera"></i> Simular Câmera
                        </button>
                        <button type="button" onclick="limparCaptura()" class="py-3 px-4 rounded-xl bg-slate-800 text-slate-300 font-semibold text-xs flex items-center justify-center gap-2 hover:bg-slate-700 transition-all">
                            <i class="fa-solid fa-rotate-left"></i> Limpar Foto
                        </button>
                    </div>
                </div>

                <!-- Formulário de Registro -->
                <div class="lg:col-span-7 bg-slate-900 border border-slate-800 rounded-3xl p-6 sm:p-8 space-y-6">
                    <h3 class="text-lg font-bold text-white border-b border-slate-800 pb-3">Dados da Credencial do Condutor</h3>

                    <form id="form-cadastro-face" onsubmit="salvarCadastroFace(event)" class="space-y-4">
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-slate-300 mb-1">Nome Completo do Condutor</label>
                                <input type="text" id="face-nome" required placeholder="Ex: Dra. Mariana Silva" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-500 transition-colors">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-300 mb-1">Tipo de Veículo / Frota</label>
                                <select id="face-tipo-veiculo" required class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-500 transition-colors">
                                    <option value="Ambulância SAMU">Ambulância SAMU</option>
                                    <option value="Corpo de Bombeiros">Corpo de Bombeiros</option>
                                    <option value="Polícia Militar/Civil">Polícia Militar/Civil</option>
                                    <option value="Ônibus Urbano Express">Ônibus Urbano Express (BRT)</option>
                                    <option value="Veículo de Defesa Civil">Veículo de Defesa Civil</option>
                                </select>
                            </div>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-slate-300 mb-1">Placa do Veículo</label>
                                <input type="text" id="face-placa" required placeholder="Ex: ABC-1D23" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-500 transition-colors">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-300 mb-1">Nível de Prioridade Desejado</label>
                                <select id="face-prioridade" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-500 transition-colors">
                                    <option value="Nível 1 - Crítico (Ambulância/Incêndio)">Nível 1 - Crítico (Zero Espera)</option>
                                    <option value="Nível 2 - Alto (Segurança Pública)">Nível 2 - Alto (Prioridade de Corredor)</option>
                                    <option value="Nível 3 - Médio (Transporte Público)">Nível 3 - Médio (Otimização de Horário)</option>
                                </select>
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-300 mb-1">Matrícula da Instituição / Registro Oficial</label>
                            <input type="text" id="face-matricula" required placeholder="Ex: MSP-99482-BR" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-500 transition-colors">
                        </div>

                        <button type="submit" class="w-full py-4 rounded-xl font-bold text-slate-950 bg-gradient-to-r from-cyan-400 to-teal-400 hover:from-cyan-300 hover:to-teal-300 transition-all shadow-lg shadow-cyan-500/20 flex items-center justify-center gap-2">
                            <i class="fa-solid fa-cloud-arrow-up"></i> Cadastrar Biometria no Sistema SmartTraffic
                        </button>
                    </form>
                </div>

            </div>

            <!-- Tabela de Condutores Cadastrados Recentemente -->
            <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 space-y-4">
                <h3 class="text-base font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-list-check text-cyan-400"></i> Veículos Autorizados e Cadastrados
                </h3>

                <div class="overflow-x-auto">
                    <table class="w-full text-left text-sm text-slate-300">
                        <thead class="bg-slate-950 text-slate-400 text-xs uppercase font-mono border-b border-slate-800">
                            <tr>
                                <th class="p-3">Condutor</th>
                                <th class="p-3">Veículo / Frota</th>
                                <th class="p-3">Placa</th>
                                <th class="p-3">Prioridade</th>
                                <th class="p-3">Status Biométrico</th>
                            </tr>
                        </thead>
                        <tbody id="lista-cadastrados-tbody" class="divide-y divide-slate-800/60">
                            <!-- Inserido dinamicamente via JS -->
                        </tbody>
                    </table>
                </div>
            </div>

        </section>

        <!-- ================= PAGE 3: GESTÃO DE PRIORIDADES ================= -->
        <section id="view-prioridades" class="app-view hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-10">
           
            <div class="border-b border-slate-800 pb-6">
                <span class="text-xs font-mono text-amber-400 uppercase tracking-widest font-semibold">Algoritmos de Tráfego Adaptativo</span>
                <h2 class="text-3xl font-extrabold text-white mt-1">Painel de Configuração de Prioridades</h2>
                <p class="text-slate-400 text-sm mt-1">Ajuste os pesos do algoritmo de IA para otimizar corredores de ônibus, pedestres PNE e viaturas de emergência.</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
               
                <!-- Sliders e Controles de Prioridade -->
                <div class="lg:col-span-8 space-y-6">
                   
                    <!-- Card 1: Socorro de Emergência -->
                    <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 space-y-4">
                        <div class="flex items-center justify-between">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 rounded-xl bg-red-500/20 text-red-400 flex items-center justify-center">
                                    <i class="fa-solid fa-truck-medical text-lg"></i>
                                </div>
                                <div>
                                    <h4 class="font-bold text-white text-base">Socorro de Emergência (SAMU / Bombeiros)</h4>
                                    <p class="text-slate-400 text-xs">Abertura imediata de semáforos no sentido do deslocamento.</p>
                                </div>
                            </div>
                            <span class="px-3 py-1 rounded-full bg-red-500/20 text-red-400 text-xs font-bold font-mono">Prioridade Máxima</span>
                        </div>
                       
                        <div class="space-y-2">
                            <div class="flex justify-between text-xs text-slate-300 font-mono">
                                <span>Tempo Extra de Sinal Verde de Emergência:</span>
                                <span id="val-tempo-emergencia" class="text-red-400 font-bold">45 seg</span>
                            </div>
                            <input type="range" min="15" max="60" value="45" oninput="atualizarPrioridades()" id="slider-emergencia" class="w-full accent-red-500 cursor-pointer">
                        </div>
                    </div>

                    <!-- Card 2: Transporte Coletivo Urbano -->
                    <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 space-y-4">
                        <div class="flex items-center justify-between">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 rounded-xl bg-amber-500/20 text-amber-400 flex items-center justify-center">
                                    <i class="fa-solid fa-bus text-lg"></i>
                                </div>
                                <div>
                                    <h4 class="font-bold text-white text-base">Transporte Coletivo (BRT & Ônibus)</h4>
                                    <p class="text-slate-400 text-xs">Extensão do verde quando houver ônibus lotados aproximando-se.</p>
                                </div>
                            </div>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" checked id="toggle-brt" onchange="atualizarPrioridades()" class="sr-only peer">
                                <div class="w-11 h-6 bg-slate-800 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-amber-500"></div>
                            </label>
                        </div>

                        <div class="space-y-2">
                            <div class="flex justify-between text-xs text-slate-300 font-mono">
                                <span>Sensibilidade do Detector de Lotação:</span>
                                <span id="val-brt-sens" class="text-amber-400 font-bold">80%</span>
                            </div>
                            <input type="range" min="30" max="100" value="80" oninput="atualizarPrioridades()" id="slider-brt" class="w-full accent-amber-500 cursor-pointer">
                        </div>
                    </div>

                    <!-- Card 3: Pedestres e Mobilidade Reduzida -->
                    <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 space-y-4">
                        <div class="flex items-center justify-between">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 rounded-xl bg-cyan-500/20 text-cyan-400 flex items-center justify-center">
                                    <i class="fa-solid fa-person-walking text-lg"></i>
                                </div>
                                <div>
                                    <h4 class="font-bold text-white text-base">Pedestres & Acessibilidade PNE</h4>
                                    <p class="text-slate-400 text-xs">Tempo adaptativo de travessia estendida para idosos e PCDs.</p>
                                </div>
                            </div>
                            <span class="px-3 py-1 rounded-full bg-cyan-500/20 text-cyan-400 text-xs font-bold font-mono">Ativado</span>
                        </div>

                        <div class="space-y-2">
                            <div class="flex justify-between text-xs text-slate-300 font-mono">
                                <span>Adicional de Segurança para Travessia:</span>
                                <span id="val-pedestre-tempo" class="text-cyan-400 font-bold">+12 seg</span>
                            </div>
                            <input type="range" min="5" max="25" value="12" oninput="atualizarPrioridades()" id="slider-pedestre" class="w-full accent-cyan-500 cursor-pointer">
                        </div>
                    </div>

                </div>

                <!-- Painel Lateral: Distribuição Visual de Tempo do Cruzamento -->
                <div class="lg:col-span-4 bg-slate-900 border border-slate-800 rounded-3xl p-6 space-y-6 flex flex-col justify-between">
                    <div>
                        <h3 class="text-base font-bold text-white border-b border-slate-800 pb-3">Distribuição do Ciclo de Tráfego</h3>
                       
                        <div class="mt-6 space-y-4">
                            <div>
                                <div class="flex justify-between text-xs text-slate-300 mb-1">
                                    <span>Via Principal (Av. Paulista)</span>
                                    <span id="p-principal" class="font-mono font-bold text-emerald-400">55%</span>
                                </div>
                                <div class="w-full h-3 bg-slate-950 rounded-full overflow-hidden">
                                    <div id="bar-principal" class="h-full bg-emerald-500 transition-all duration-300" style="width: 55%;"></div>
                                </div>
                            </div>

                            <div>
                                <div class="flex justify-between text-xs text-slate-300 mb-1">
                                    <span>Via Secundária (Travessa)</span>
                                    <span id="p-secundarias" class="font-mono font-bold text-cyan-400">25%</span>
                                </div>
                                <div class="w-full h-3 bg-slate-950 rounded-full overflow-hidden">
                                    <div id="bar-secundarias" class="h-full bg-cyan-500 transition-all duration-300" style="width: 25%;"></div>
                                </div>
                            </div>

                            <div>
                                <div class="flex justify-between text-xs text-slate-300 mb-1">
                                    <span>Buffer de Prioridade / Emergência</span>
                                    <span id="p-buffer" class="font-mono font-bold text-amber-400">20%</span>
                                </div>
                                <div class="w-full h-3 bg-slate-950 rounded-full overflow-hidden">
                                    <div id="bar-buffer" class="h-full bg-amber-500 transition-all duration-300" style="width: 20%;"></div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="bg-slate-950 p-4 rounded-2xl border border-slate-800 space-y-2">
                        <span class="text-xs font-mono text-slate-400 block">STATUS DO ALGORITMO:</span>
                        <div class="text-xs text-emerald-400 font-mono">
                            ✓ Balanceamento em tempo real ativo. Nenhuma retenção detectada.
                        </div>
                        <button onclick="salvarPrioridades()" class="w-full mt-2 py-3 rounded-xl bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold text-xs transition-colors shadow-lg shadow-amber-500/20">
                            Aplicar Alterações nos Semáforos
                        </button>
                    </div>
                </div>

            </div>

        </section>

        <!-- ================= PAGE 4: CONTATO ================= -->
        <section id="view-contato" class="app-view hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-10">
           
            <div class="border-b border-slate-800 pb-6">
                <span class="text-xs font-mono text-emerald-400 uppercase tracking-widest font-semibold">Central de Atendimento SmartTraffic</span>
                <h2 class="text-3xl font-extrabold text-white mt-1">Fale com Nossos Engenheiros de Tráfego</h2>
                <p class="text-slate-400 text-sm mt-1">Dúvidas técnicas, solicitações de expansão para a sua cidade ou suporte emergencial a prefeituras.</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
               
                <!-- Formulário de Contato -->
                <div class="lg:col-span-7 bg-slate-900 border border-slate-800 rounded-3xl p-6 sm:p-8 space-y-6">
                    <form id="form-contato" onsubmit="enviarContato(event)" class="space-y-4">
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-slate-300 mb-1">Seu Nome</label>
                                <input type="text" required placeholder="Ex: Roberto Mendes" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-emerald-500 transition-colors">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-300 mb-1">E-mail Corporativo ou Pessoal</label>
                                <input type="email" required placeholder="Ex: roberto@mobilidade.gov.br" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-emerald-500 transition-colors">
                            </div>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-slate-300 mb-1">Órgão / Empresa / Município</label>
                                <input type="text" placeholder="Ex: Prefeitura de São Paulo - SMT" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-emerald-500 transition-colors">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-300 mb-1">Assunto do Contato</label>
                                <select class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-emerald-500 transition-colors">
                                    <option>Implementação de Semáforo Inteligente em Município</option>
                                    <option>Suporte Técnico de Semáforos</option>
                                    <option>Parceria em Engenharia de Tráfego</option>
                                    <option>Outros Assuntos</option>
                                </select>
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-300 mb-1">Mensagem</label>
                            <textarea rows="4" required placeholder="Descreva os detalhes da sua solicitação..." class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-emerald-500 transition-colors"></textarea>
                        </div>

                        <button type="submit" class="w-full py-4 rounded-xl font-bold text-slate-950 bg-gradient-to-r from-emerald-400 to-teal-400 hover:from-emerald-300 hover:to-teal-300 transition-all shadow-lg shadow-emerald-500/20 flex items-center justify-center gap-2">
                            <i class="fa-solid fa-paper-plane"></i> Enviar Mensagem para a Central
                        </button>
                    </form>
                </div>

                <!-- Info Lateral & Mapa Ilustrativo -->
                <div class="lg:col-span-5 space-y-6">
                    <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 space-y-4">
                        <h3 class="text-base font-bold text-white border-b border-slate-800 pb-3">Canais de Atendimento Direto</h3>

                        <div class="space-y-4 text-sm">
                            <div class="flex items-start gap-3">
                                <div class="w-9 h-9 rounded-xl bg-emerald-500/10 text-emerald-400 flex items-center justify-center shrink-0">
                                    <i class="fa-solid fa-phone"></i>
                                </div>
                                <div>
                                    <span class="block font-bold text-white">Central 24 Horas</span>
                                    <span class="text-slate-400 font-mono">0800 777 SMART (76278)</span>
                                </div>
                            </div>

                            <div class="flex items-start gap-3">
                                <div class="w-9 h-9 rounded-xl bg-emerald-500/10 text-emerald-400 flex items-center justify-center shrink-0">
                                    <i class="fa-solid fa-envelope"></i>
                                </div>
                                <div>
                                    <span class="block font-bold text-white">E-mail de Suporte</span>
                                    <span class="text-slate-400 font-mono">suporte@smarttraffic.ai</span>
                                </div>
                            </div>

                            <div class="flex items-start gap-3">
                                <div class="w-9 h-9 rounded-xl bg-emerald-500/10 text-emerald-400 flex items-center justify-center shrink-0">
                                    <i class="fa-solid fa-building"></i>
                                </div>
                                <div>
                                    <span class="block font-bold text-white">Centro de Comando de IA</span>
                                    <span class="text-slate-400">Av. das Nações Unidas, 12901 - São Paulo/SP</span>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Ilustração de Mapa Digital -->
                    <div class="bg-slate-950 border border-slate-800 rounded-3xl p-6 relative overflow-hidden flex flex-col items-center justify-center text-center space-y-3 min-h-[180px]">
                        <div class="w-12 h-12 rounded-full bg-emerald-500/20 text-emerald-400 flex items-center justify-center text-2xl animate-bounce">
                            <i class="fa-solid fa-map-location-dot"></i>
                        </div>
                        <h4 class="font-bold text-white text-sm">Central Operacional Monitorando 1.420 Semáforos</h4>
                        <p class="text-xs text-slate-500 font-mono">Status da Rede: Cobertura de 99.9% em tempo real</p>
                    </div>
                </div>

            </div>

        </section>

        <!-- ================= PAGE 5: FEEDBACK & AVALIAÇÕES ================= -->
        <section id="view-feedback" class="app-view hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 space-y-10">
           
            <div class="border-b border-slate-800 pb-6">
                <span class="text-xs font-mono text-purple-400 uppercase tracking-widest font-semibold">Participação Cidadã</span>
                <h2 class="text-3xl font-extrabold text-white mt-1">Feedback do Cidadão & Relatos de Trânsito</h2>
                <p class="text-slate-400 text-sm mt-1">Avalie a precisão dos semáforos no seu bairro e ajude nosso algoritmo a evoluir continuamente.</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
               
                <!-- Formulário de Feedback -->
                <div class="lg:col-span-5 bg-slate-900 border border-slate-800 rounded-3xl p-6 space-y-6">
                    <h3 class="text-lg font-bold text-white border-b border-slate-800 pb-3">Deixe sua Avaliação</h3>

                    <form id="form-feedback" onsubmit="salvarFeedback(event)" class="space-y-4">
                        <div>
                            <label class="block text-xs font-semibold text-slate-300 mb-1">Seu Nome / Apelido</label>
                            <input type="text" id="fb-nome" required placeholder="Ex: Carlos E." class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-purple-500 transition-colors">
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-300 mb-1">Cruzamento ou Bairro</label>
                            <input type="text" id="fb-local" required placeholder="Ex: Av. Faria Lima x Rebouças" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-purple-500 transition-colors">
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-300 mb-1">Avaliação do Semáforo Inteligente</label>
                            <div class="flex items-center gap-2 text-2xl text-slate-600 py-1" id="star-rating-container">
                                <i class="fa-solid fa-star cursor-pointer hover:text-amber-400 transition-colors" onclick="setRating(1)"></i>
                                <i class="fa-solid fa-star cursor-pointer hover:text-amber-400 transition-colors" onclick="setRating(2)"></i>
                                <i class="fa-solid fa-star cursor-pointer hover:text-amber-400 transition-colors" onclick="setRating(3)"></i>
                                <i class="fa-solid fa-star cursor-pointer hover:text-amber-400 transition-colors" onclick="setRating(4)"></i>
                                <i class="fa-solid fa-star cursor-pointer hover:text-amber-400 transition-colors" onclick="setRating(5)"></i>
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-300 mb-1">Seu Comentário ou Relato</label>
                            <textarea id="fb-comentario" rows="3" required placeholder="Ex: O tempo verde para os pedestres melhorou muito a travessia..." class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-purple-500 transition-colors"></textarea>
                        </div>

                        <button type="submit" class="w-full py-3.5 rounded-xl font-bold text-slate-950 bg-gradient-to-r from-purple-400 to-indigo-400 hover:from-purple-300 hover:to-indigo-300 transition-all shadow-lg shadow-purple-500/20 flex items-center justify-center gap-2">
                            <i class="fa-solid fa-star"></i> Publicar Feedback
                        </button>
                    </form>
                </div>

                <!-- Feed de Comentários / Avaliações -->
                <div class="lg:col-span-7 space-y-4">
                    <div class="flex items-center justify-between">
                        <h3 class="text-base font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-comments text-purple-400"></i> Feed de Avaliações Recentes
                        </h3>
                        <span class="text-xs font-mono text-slate-500">Atualizado agora</span>
                    </div>

                    <div id="feedback-list-container" class="space-y-4 max-h-[600px] overflow-y-auto pr-1">
                        <!-- Inserido dinamicamente via Javascript -->
                    </div>
                </div>

            </div>

        </section>

    </main>

    <!-- RODAPÉ (FOOTER) -->
    <footer class="bg-slate-950 border-t border-slate-900 py-10 mt-20">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col md:flex-row items-center justify-between gap-6">
            <div class="flex items-center gap-3">
                <div class="w-9 h-9 rounded-lg bg-emerald-500/20 flex items-center justify-center text-emerald-400">
                    <i class="fa-solid fa-traffic-light"></i>
                </div>
                <div>
                    <span class="font-extrabold text-white">SmartTraffic AI</span>
                    <p class="text-xs text-slate-500">© 2026 SmartTraffic AI Inc. Todos os direitos reservados.</p>
                </div>
            </div>

            <div class="flex items-center gap-6 text-xs text-slate-400">
                <button onclick="navigateTo('home')" class="hover:text-emerald-400 transition-colors">Início</button>
                <button onclick="navigateTo('cadastro')" class="hover:text-cyan-400 transition-colors">Biometria Facial</button>
                <button onclick="navigateTo('prioridades')" class="hover:text-amber-400 transition-colors">Prioridades</button>
                <button onclick="navigateTo('contato')" class="hover:text-emerald-400 transition-colors">Suporte 24h</button>
                <button onclick="navigateTo('feedback')" class="hover:text-purple-400 transition-colors">Feedback</button>
            </div>
        </div>
    </footer>

    <!-- LÓGICA JAVASCRIPT COMPLETA -->
    <script>
        /* ==========================================================================
           1. ESTADO E ROTEAMENTO DA APLICAÇÃO (SPA)
           ========================================================================== */
        let currentRating = 5;
        let activePage = 'home';

        function navigateTo(pageId) {
            // Oculta todas as páginas
            document.querySelectorAll('.app-view').forEach(view => view.classList.add('hidden'));
           
            // Exibe a página selecionada
            const targetView = document.getElementById(`view-${pageId}`);
            if (targetView) {
                targetView.classList.remove('hidden');
                activePage = pageId;
            }

            // Atualiza botões da navegação desktop
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('text-emerald-400', 'bg-emerald-500/10', 'border', 'border-emerald-500/30');
                btn.classList.add('text-slate-300');
            });

            const activeNavBtn = document.getElementById(`nav-${pageId}`);
            if (activeNavBtn) {
                activeNavBtn.classList.remove('text-slate-300');
                activeNavBtn.classList.add('text-emerald-400', 'bg-emerald-500/10', 'border', 'border-emerald-500/30');
            }

            // Scroll para o topo
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function toggleMobileMenu() {
            const menu = document.getElementById('mobile-menu');
            menu.classList.toggle('hidden');
        }

        function scrollToSimulator() {
            const el = document.getElementById('simulator-section');
            if (el) el.scrollIntoView({ behavior: 'smooth' });
        }

        /* ==========================================================================
           2. SISTEMA DE TOAST NOTIFICAÇÕES (MENSAGENS BONITAS)
           ========================================================================== */
        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
           
            let iconClass = 'fa-circle-info text-cyan-400';
            let borderColor = 'border-cyan-500/40';

            if (type === 'success') {
                iconClass = 'fa-circle-check text-emerald-400';
                borderColor = 'border-emerald-500/40';
            } else if (type === 'warning') {
                iconClass = 'fa-triangle-exclamation text-amber-400';
                borderColor = 'border-amber-500/40';
            } else if (type === 'emergency') {
                iconClass = 'fa-truck-medical text-rose-400 animate-pulse';
                borderColor = 'border-rose-500/60';
            }

            toast.className = `pointer-events-auto bg-slate-900/95 border ${borderColor} text-slate-100 px-4 py-3 rounded-2xl shadow-2xl flex items-center gap-3 text-xs sm:text-sm font-medium transition-all duration-300 transform translate-y-4 opacity-0`;
            toast.innerHTML = `
                <i class="fa-solid ${iconClass} text-lg"></i>
                <span>${message}</span>
            `;

            container.appendChild(toast);

            // Animação de entrada
            setTimeout(() => {
                toast.classList.remove('translate-y-4', 'opacity-0');
            }, 10);

            // Remoção automática
            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-4');
                setTimeout(() => toast.remove(), 300);
            }, 4000);
        }

        /* ==========================================================================
           3. MOTOR DO SIMULADOR EM TEMPO REAL DE SEMÁFORO
           ========================================================================== */
        let simTimerValue = 12;
        let currentSimState = 'GREEN'; // GREEN, YELLOW, RED, EMERGENCY
        let simInterval = null;

        function initSimulator() {
            updateSimVisuals();
            if (simInterval) clearInterval(simInterval);
           
            simInterval = setInterval(() => {
                if (currentSimState === 'EMERGENCY') return; // Pausado para emergência

                simTimerValue--;
                document.getElementById('sim-timer').innerText = simTimerValue;

                if (simTimerValue <= 0) {
                    switchSimPhase();
                }
            }, 1000);
        }

        function switchSimPhase() {
            if (currentSimState === 'GREEN') {
                currentSimState = 'YELLOW';
                simTimerValue = 3;
            } else if (currentSimState === 'YELLOW') {
                currentSimState = 'RED';
                simTimerValue = 14;
            } else if (currentSimState === 'RED') {
                currentSimState = 'GREEN';
                simTimerValue = 15;
            }
            updateSimVisuals();
        }

        function updateSimVisuals() {
            const ledRed = document.getElementById('led-red');
            const ledYellow = document.getElementById('led-yellow');
            const ledGreen = document.getElementById('led-green');
            const timerEl = document.getElementById('sim-timer');
            const aiLog = document.getElementById('sim-ai-log');
            const vehicleTitle = document.getElementById('sim-vehicle-title');
            const vehicleDesc = document.getElementById('sim-vehicle-desc');
            const vehicleIcon = document.getElementById('sim-vehicle-icon');
            const vehicleBox = document.getElementById('sim-vehicle-box');

            // Reset leds
            ledRed.classList.remove('active');
            ledYellow.classList.remove('active');
            ledGreen.classList.remove('active');

            if (currentSimState === 'GREEN') {
                ledGreen.classList.add('active');
                timerEl.className = 'text-4xl font-black font-mono text-emerald-400';
                aiLog.innerText = '> IA: Fase VERDE ativada. Tráfego contínuo na artéria principal.';
                vehicleTitle.innerText = 'Fluxo Contínuo de Veículos';
                vehicleDesc.innerText = 'Velocidade Média: 52 km/h';
                vehicleIcon.className = 'fa-solid fa-car';
                vehicleBox.className = 'relative z-20 flex items-center gap-3 bg-emerald-950/80 border border-emerald-500/50 p-2.5 rounded-xl shadow-lg';
            } else if (currentSimState === 'YELLOW') {
                ledYellow.classList.add('active');
                timerEl.className = 'text-4xl font-black font-mono text-amber-400';
                aiLog.innerText = '> IA: Fase AMARELA. Desaceleração gradual detectada pelos sensores.';
                vehicleTitle.innerText = 'Desaceleração';
                vehicleDesc.innerText = 'Veículos reduzindo velocidade';
                vehicleIcon.className = 'fa-solid fa-triangle-exclamation';
                vehicleBox.className = 'relative z-20 flex items-center gap-3 bg-amber-950/80 border border-amber-500/50 p-2.5 rounded-xl shadow-lg';
            } else if (currentSimState === 'RED') {
                ledRed.classList.add('active');
                timerEl.className = 'text-4xl font-black font-mono text-red-400';
                aiLog.innerText = '> IA: Fase VERMELHA. Retenção ativa. Liberada travessia lateral/pedestres.';
                vehicleTitle.innerText = 'Veículos Parados';
                vehicleDesc.innerText = 'Fila sob controle (3 carros)';
                vehicleIcon.className = 'fa-solid fa-hand';
                vehicleBox.className = 'relative z-20 flex items-center gap-3 bg-red-950/80 border border-red-500/50 p-2.5 rounded-xl shadow-lg';
            }
        }

        // Triggers de Eventos
        function triggerEmergencyEvent() {
            currentSimState = 'EMERGENCY';
            const ledRed = document.getElementById('led-red');
            const ledYellow = document.getElementById('led-yellow');
            const ledGreen = document.getElementById('led-green');

            ledRed.classList.remove('active');
            ledYellow.classList.remove('active');
            ledGreen.classList.add('active'); // Força verde

            document.getElementById('sim-timer').innerText = '45';
            document.getElementById('sim-ai-log').innerText = '🚨 OVERRIDE DE EMERGÊNCIA: Ambulância SAMU detectada por visão computacional! Corredor liberado.';
            document.getElementById('sim-vehicle-title').innerText = 'SAMU - Unidade 102';
            document.getElementById('sim-vehicle-desc').innerText = 'Match Biométrico: 99.8% (Dra. Mariana)';
            document.getElementById('sim-vehicle-icon').className = 'fa-solid fa-truck-medical animate-bounce text-red-400';
            document.getElementById('sim-vehicle-box').className = 'relative z-20 flex items-center gap-3 bg-red-950/90 border border-red-500 p-2.5 rounded-xl shadow-2xl shadow-red-500/40';

            showToast('🚨 Emergência Detectada! Sinal retido em VERDE para a Ambulância.', 'emergency');

            // Retorna ao ciclo normal após 6 segundos
            setTimeout(() => {
                currentSimState = 'GREEN';
                simTimerValue = 10;
                updateSimVisuals();
                showToast('Emergência concluída. Retornando ao ciclo adaptativo.', 'success');
            }, 6000);
        }

        function triggerTrafficPeakEvent() {
            if (currentSimState === 'GREEN') {
                simTimerValue += 10;
                document.getElementById('sim-timer').innerText = simTimerValue;
                showToast('⚡ IA detectou alta densidade! Adicionados +10s de tempo verde.', 'warning');
                document.getElementById('sim-ai-log').innerText = '> IA: Congestionamento detectado na via. Adicionando tempo extra de abertura.';
            } else {
                showToast('Solicitação recebida. A IA estenderá a próxima fase verde.', 'info');
            }
        }

        function triggerPedestrianEvent() {
            showToast('🚶 Pedestre PNE detectado. Travessia segura priorizada na próxima fase.', 'info');
            document.getElementById('sim-ai-log').innerText = '> IA: Botão/Visão de Pedestre acionada. Tempo de travessia estendido em +12s.';
        }

        /* ==========================================================================
           4. MÓDULO DE CADASTRO FACIAL SIMULADO
           ========================================================================== */
        const mockCadastrados = [
            { condutor: 'Dra. Mariana Silva', veiculo: 'Ambulância SAMU', placa: 'SAM-0192', prioridade: 'Nível 1 - Crítico', status: 'Biometria Ativa (99.8%)' },
            { condutor: 'Capitão Ricardo Alves', veiculo: 'Corpo de Bombeiros', placa: 'CBM-1930', prioridade: 'Nível 1 - Crítico', status: 'Biometria Ativa (99.5%)' },
            { condutor: 'Marcos Oliveira', veiculo: 'Ônibus BRT Express', placa: 'BRT-5501', prioridade: 'Nível 3 - Médio', status: 'Biometria Ativa (98.9%)' }
        ];

        function renderCadastrados() {
            const tbody = document.getElementById('lista-cadastrados-tbody');
            tbody.innerHTML = '';

            mockCadastrados.forEach(item => {
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-slate-800/40 transition-colors';
                tr.innerHTML = `
                    <td class="p-3 font-semibold text-white flex items-center gap-2">
                        <i class="fa-solid fa-circle-user text-cyan-400"></i> ${item.condutor}
                    </td>
                    <td class="p-3">${item.veiculo}</td>
                    <td class="p-3 font-mono text-xs">${item.placa}</td>
                    <td class="p-3"><span class="px-2 py-0.5 rounded bg-cyan-500/20 text-cyan-300 text-xs">${item.prioridade}</span></td>
                    <td class="p-3 font-mono text-xs text-emerald-400">✓ ${item.status}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function simularCapturaCamera() {
            const reticle = document.getElementById('scanner-reticle');
            const overlay = document.getElementById('camera-overlay');
            const photo = document.getElementById('captured-photo');
            const badge = document.getElementById('camera-status-badge');

            reticle.classList.remove('hidden');
            overlay.classList.add('hidden');
           
            // Define uma imagem ilustrativa de motorista
            photo.src = 'https://images.unsplash.com/photo-1560250097-0b93528c311a?auto=format&fit=crop&w=400&q=80';
            photo.classList.remove('hidden');
            badge.innerText = '● ROSTO DETECTADO (99.8%)';
            badge.className = 'text-cyan-400 animate-pulse';

            showToast('Scan facial realizado com sucesso!', 'success');
        }

        function limparCaptura() {
            document.getElementById('scanner-reticle').classList.add('hidden');
            document.getElementById('captured-photo').classList.add('hidden');
            document.getElementById('camera-overlay').classList.remove('hidden');
            document.getElementById('camera-status-badge').innerText = '● PRONTO';
            document.getElementById('camera-status-badge').className = 'text-emerald-400';
        }

        function salvarCadastroFace(e) {
            e.preventDefault();
            const nome = document.getElementById('face-nome').value;
            const tipo = document.getElementById('face-tipo-veiculo').value;
            const placa = document.getElementById('face-placa').value;
            const prioridade = document.getElementById('face-prioridade').value;

            mockCadastrados.unshift({
                condutor: nome,
                veiculo: tipo,
                placa: placa,
                prioridade: prioridade,
                status: 'Biometria Ativa (99.9%)'
            });

            renderCadastrados();
            document.getElementById('form-cadastro-face').reset();
            limparCaptura();
            showToast(`Biometria de ${nome} cadastrada na rede SmartTraffic!`, 'success');
        }

        /* ==========================================================================
           5. MÓDULO DE GESTÃO DE PRIORIDADES
           ========================================================================== */
        function atualizarPrioridades() {
            const tempEmerg = document.getElementById('slider-emergencia').value;
            const sensBRT = document.getElementById('slider-brt').value;
            const tempPed = document.getElementById('slider-pedestre').value;

            document.getElementById('val-tempo-emergencia').innerText = `${tempEmerg} seg`;
            document.getElementById('val-brt-sens').innerText = `${sensBRT}%`;
            document.getElementById('val-pedestre-tempo').innerText = `+${tempPed} seg`;

            // Recalcula visualização gráfica mock
            const pPrincipal = Math.max(30, 70 - Math.floor(tempPed / 2));
            const pSec = Math.max(15, 100 - pPrincipal - Math.floor(tempEmerg / 3));
            const pBuffer = 100 - pPrincipal - pSec;

            document.getElementById('p-principal').innerText = `${pPrincipal}%`;
            document.getElementById('bar-principal').style.width = `${pPrincipal}%`;

            document.getElementById('p-secundarias').innerText = `${pSec}%`;
            document.getElementById('bar-secundarias').style.width = `${pSec}%`;

            document.getElementById('p-buffer').innerText = `${pBuffer}%`;
            document.getElementById('bar-buffer').style.width = `${pBuffer}%`;
        }

        function salvarPrioridades() {
            showToast('Novos parâmetros de tempo enviados para os controladores viários.', 'success');
        }

        /* ==========================================================================
           6. MÓDULO DE CONTATO
           ========================================================================== */
        function enviarContato(e) {
            e.preventDefault();
            e.target.reset();
            showToast('Mensagem enviada com sucesso! Nossa central entrará em contato.', 'success');
        }

        /* ==========================================================================
           7. MÓDULO DE FEEDBACK & AVALIAÇÕES
           ========================================================================== */
        const mockFeedbacks = [
            { nome: 'Fernando Prado', local: 'Av. Paulista x Rebouças', rating: 5, texto: 'Sensacional a redução na fila do semáforo no horário de pico das 18h.', data: 'Há 10 minutos' },
            { nome: 'Beatriz Lima', local: 'Rua Consolação 1400', rating: 4, texto: 'Excelente para idosos! Consegui atravessar a via com tranquilidade.', data: 'Há 1 hora' },
            { nome: 'Luciana M.', local: 'Av. Brigadeiro Faria Lima', rating: 5, texto: 'Vi a ambulância passando e o sinal abriu instantaneamente na frente dela.', data: 'Há 3 horas' }
        ];

        function setRating(val) {
            currentRating = val;
            const stars = document.getElementById('star-rating-container').children;
            for (let i = 0; i < stars.length; i++) {
                if (i < val) {
                    stars[i].className = 'fa-solid fa-star cursor-pointer text-amber-400 transition-colors';
                } else {
                    stars[i].className = 'fa-solid fa-star cursor-pointer text-slate-700 transition-colors';
                }
            }
        }

        function renderFeedbacks() {
            const container = document.getElementById('feedback-list-container');
            container.innerHTML = '';

            mockFeedbacks.forEach(fb => {
                const card = document.createElement('div');
                card.className = 'glass-panel p-5 rounded-2xl border border-slate-800 space-y-2';
               
                let starsHTML = '';
                for (let i = 0; i < 5; i++) {
                    if (i < fb.rating) starsHTML += '<i class="fa-solid fa-star text-amber-400 text-xs"></i>';
                    else starsHTML += '<i class="fa-solid fa-star text-slate-700 text-xs"></i>';
                }

                card.innerHTML = `
                    <div class="flex justify-between items-start">
                        <div>
                            <h4 class="font-bold text-white text-sm">${fb.nome}</h4>
                            <span class="text-xs text-slate-400"><i class="fa-solid fa-location-dot text-purple-400 mr-1"></i>${fb.local}</span>
                        </div>
                        <div class="flex items-center gap-1">${starsHTML}</div>
                    </div>
                    <p class="text-xs text-slate-300 leading-relaxed">${fb.texto}</p>
                    <span class="text-[10px] font-mono text-slate-500 block pt-1">${fb.data} • Validado via Cidadão Conectado</span>
                `;
                container.appendChild(card);
            });
        }

        function salvarFeedback(e) {
            e.preventDefault();
            const nome = document.getElementById('fb-nome').value;
            const local = document.getElementById('fb-local').value;
            const comentario = document.getElementById('fb-comentario').value;

            mockFeedbacks.unshift({
                nome: nome,
                local: local,
                rating: currentRating,
                texto: comentario,
                data: 'Agora mesmo'
            });

            renderFeedbacks();
            document.getElementById('form-feedback').reset();
            setRating(5);
            showToast('Obrigado pelo seu feedback! Ele ajuda a treinar nossa IA.', 'success');
        }

        /* ==========================================================================
           INICIALIZAÇÃO AO CARREGAR A PÁGINA
           ========================================================================== */
        window.onload = function() {
            initSimulator();
            renderCadastrados();
            renderFeedbacks();
            setRating(5);
            atualizarPrioridades();
        };
    </script>
</body>
</html>
