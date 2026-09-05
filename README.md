# Foro-acad-mico-5
\documentclass[12pt,a4paper]{article}

% --- PAQUETES DE CONFIGURACIÓN Y IDIOMA ---
\usepackage[spanish]{babel}
\usepackage[utf8]{utf8}
\usepackage[margin=2.5cm]{geometry}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{listings}
\usepackage{xcolor}
\usepackage{booktabs}
\usepackage{fancyhdr}

% --- CONFIGURACIÓN DE APARIENCIA Y CÓDIGO ---
\hypersetup{
    colorlinks=true,
    linkcolor=blue,
    filecolor=magenta,      
    urlcolor=cyan,
    pdftitle={Informe Técnico: Infraestructura y Microservicios},
}

\definecolor{codegreen}{rgb}{0,0.6,0}
\definecolor{codegray}{rgb}{0.5,0.5,0.5}
\definecolor{codepurple}{rgb}{0.58,0,0.82}
\definecolor{backcolour}{rgb}{0.95,0.95,0.92}

\lstdefinestyle{codeStyle}{
    backgroundcolor=\color{backcolour},   
    commentstyle=\color{codegreen},
    keywordstyle=\color{magenta},
    numberstyle=\tiny\color{codegray},
    stringstyle=\color{codepurple},
    basicstyle=\ttfamily\footnotesize,
    breakatwhitespace=false,         
    breaklines=true,                 
    captionpos=b,                    
    keepspaces=true,                 
    numbers=left,                    
    numbersep=5pt,                  
    showspaces=false,                
    showstringspaces=false,
    showtabs=false,                  
    tabsize=2
}
\lstset{style=codeStyle}

% --- ENCABEZADOS Y PIES DE PÁGINA ---
\pagestyle{fancy}
\fancyhf{}
\rhead{Informe Técnico: Arquitectura y Nube}
\lhead{Orquestación y Microservicios}
\rfoot{Página \thepage}

% --- DATOS DEL DOCUMENTO ---
\title{
    \textbf{Informe Técnico: Orquestación de Servidores, Kubernetes, Microservicios, OAuth 2.0 e Implementación en la Nube}\\
    \large Análisis Arquitectónico e Implementación
}
\author{\textbf{Tu Nombre / Nombre de la Organización}}
\date{\today}

\begin{document}

% --- PORTADA ---
\maketitle
\thispagestyle{empty}
\begin{abstract}
Este documento presenta un análisis integral sobre el diseño, despliegue y seguridad de plataformas modernas en la nube. Se abordan los conceptos fundamentales de la arquitectura de microservicios, la orquestación de contenedores mediante Kubernetes, la implementación de esquemas de autenticación y autorización con OAuth 2.0, y las mejores prácticas para el despliegue en entornos Cloud.
\end{abstract}

\vfill
\tableofcontents
\newpage

% --- SECCIÓN 1 ---
\section{Introducción}
El crecimiento de los sistemas distribuidos ha cambiado la forma en que se diseñan e implementan las aplicaciones. La transición desde arquitecturas monolíticas hacia sistemas orientados a microservicios requiere el uso de herramientas especializadas para la orquestación y gestión de infraestructura.

% --- SECCIÓN 2 ---
\section{Arquitectura de Microservicios}
La arquitectura de microservicios divide una aplicación en un conjunto de servicios independientes y acoplados de forma débil.

\subsection{Principios Clave}
\begin{itemize}
    \item \textbf{Despliegue independiente:} Cada servicio puede ser desplegado sin afectar al resto del sistema.
    \item \textbf{Descentralización de datos:} Cada microservicio gestiona su propia base de datos.
    \item \textbf{Comunicación ligera:} Uso de protocolos como HTTP/REST, gRPC o mensajería asíncrona (RabbitMQ, Kafka).
\end{itemize}

% --- SECCIÓN 3 ---
\section{Orquestación de Servidores y Kubernetes}
La gestión manual de contenedores a gran escala resulta inviable. Es en este punto donde las herramientas de orquestación juegan un rol crítico.

\subsection{¿Qué es Kubernetes (K8s)?}
Kubernetes es una plataforma de código abierto diseñada para automatizar el despliegue, el escalado y la administración de aplicaciones en contenedores.

\subsection{Componentes Principales}
\begin{description}
    \item[Pods:] La unidad ejecutable más pequeña en Kubernetes.
    \item[Deployments:] Declaran el estado deseado para los Pods (réplicas, versiones).
    \item[Services:] Definen el acceso de red a un conjunto de Pods.
    \item[Ingress:] Gestiona el acceso externo a los servicios dentro del clúster (HTTP/HTTPS).
\end{description}

\subsection{Ejemplo de Configuración (Deployment en YAML)}
\begin{lstlisting}[language=XML, caption={Manifiesto básico de Deployment en Kubernetes}]
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mi-microservicio
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mi-microservicio
  template:
    metadata:
      labels:
        app: mi-microservicio
    spec:
      containers:
      - name: app
        image: mi-registro/mi-app:v1.0
        ports:
        - containerPort: 8080
\end{lstlisting}

% --- SECCIÓN 4 ---
\section{Seguridad y Autenticación con OAuth 2.0}
Para proteger la comunicación entre usuarios y microservicios, se utiliza el estándar \textbf{OAuth 2.0} junto con **JSON Web Tokens (JWT)**.

\subsection{Flujo de Trabajo de OAuth 2.0}
\begin{enumerate}
    \item El cliente solicita autorización al \textit{Authorization Server}.
    \item El usuario autentica su identidad y otorga permisos.
    \item El \textit{Authorization Server} emite un \textit{Access Token} (JWT).
    \item El cliente realiza peticiones al \textit{Resource Server} enviando el Token en la cabecera HTTP \texttt{Authorization: Bearer <token>}.
\end{enumerate}

% --- SECCIÓN 5 ---
\section{Implementación de Servidores en la Nube}
El despliegue en entornos de proveedores Cloud (AWS, Google Cloud, Azure) permite aprovechar modelos de alta disponibilidad y tolerancia a fallos.

\subsection{Modelos de Despliegue}
\begin{table}[h!]
\centering
\begin{tabular}{@{}lll@{}}
\toprule
\textbf{Proveedor} & \textbf{Servicio K8s Gestionado} & \textbf{Servicio Serverless} \\ \midrule
AWS                & EKS (Elastic Kubernetes Service) & AWS Lambda / Fargate        \\
Google Cloud (GCP) & GKE (Google Kubernetes Engine)  & Cloud Run                   \\
Azure              & AKS (Azure Kubernetes Service)   & Container Apps              \\ \bottomrule
\end{tabular}
\caption{Comparativa de servicios en la nube.}
\end{table}

\subsection{Estrategia de Integración Continua (CI/CD)}
Para automatizar el despliegue se sugiere un flujo estructurado:
\begin{itemize}
    \item \textbf{Integración:} Pruebas unitarias y construcción de imágenes Docker.
    \item \textbf{Registro:} Publicación de imágenes en un registro privado (ECR, GCR, Docker Hub).
    \item \textbf{Despliegue:} Actualización de manifiestos mediante herramientas GitOps como ArgoCD o Flux.
\end{itemize}

% --- SECCIÓN 6 ---
\section{Conclusiones}
La combinación de microservicios, Kubernetes, OAuth 2.0 y servicios en la nube proporciona un marco robusto y escalable para aplicaciones modernas. Aunque introduce complejidad operativa, el uso de herramientas de orquestación y automatización reduce considerablemente los riesgos en producción.

\end{document}
