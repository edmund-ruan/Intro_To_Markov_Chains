\documentclass{beamer}
\usepackage[utf8]{inputenc}
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{amsfonts}
\usepackage{lmodern}
\usepackage{graphicx}
\usepackage{bm}
\usepackage{bbm}
\usepackage{dsfont}

\usetheme{Madrid}

\usecolortheme{default}
\setbeamertemplate{navigation symbols}{}

% \definecolor{darkcolumbia}{RGB}{93, 167, 208}
% \setbeamercolor{structure}{fg=darkcolumbia}

\setbeamertemplate{items}[square]

\setbeamertemplate{footline}
{
\leavevmode%
\hbox{%
\begin{beamercolorbox}[wd=.4\paperwidth,ht=2.25ex,dp=1ex,center]{author in head/foot}%
\usebeamerfont{author in head/foot}\insertshortauthor
\end{beamercolorbox}%
\begin{beamercolorbox}[wd=.6\paperwidth,ht=2.25ex,dp=1ex,center]{title in head/foot}%
\usebeamerfont{title in head/foot}\insertshorttitle\hspace*{3em}
\insertframenumber{} / \inserttotalframenumber\hspace*{1ex}
\end{beamercolorbox}}%
\vskip0pt%
}
\newcommand{\N}{\mathbb{N}}
\newcommand{\Z}{\mathbb{Z}}
\newcommand{\R}{\mathbb{R}}
\newcommand{\Pb}{\mathbb{P}}
\newcommand{\Ss}{\mathbb{S}}

\AtBeginSection[]
{
\begin{frame}
\frametitle{Outline}
\tableofcontents[currentsection]
\end{frame}
}

\title{Introduction to Markov Chains}
\author{Edmund Ruan}
\date{DRP Presentation, April 28, 2021 \newline Mentor: Greyson Potter}

\begin{document}

\begin{frame}
\titlepage
\end{frame}
\begin{frame}
\frametitle{Outline}
\tableofcontents
\end{frame}

% \begin{frame}{Outline}

% \begin{enumerate}
% \item definition of a Markov chain
% \begin{itemize}
% \item Basics of probability rv, discrete time stochastic process seq of a family of rv
% \end{itemize}
% \item Transition Matrices
% \item Examples
% \begin{itemize}
% \item Gambling Problem
% \item Left + Right (random walks)
% \item Time evolution of two-state chain (simplest example of MC)
% \end{itemize}
% \item Further study ... First step analysis, stickiness
% \end{enumerate}

% \end{frame}
\section{What is a Markov chain?}
\begin{frame}{Basic Probability}
% \begin{definition}
% A random variable $X$ is a map from a ``probability space'' to the real numbers.
% \end{definition}
% For example ...

    % $$X:\{1,2,3,4,5,6\}\times \{1,2,3,4,5,6\}\rightarrow \R$$
    % $$(x,y)\mapsto x+y$$
    \begin{definition}[Probability Measure]
    A probability measure $\Pb$ is an $\R$-valued function on a set of ``possible events.''
    \end{definition}

    For example if $Z_k$ is a random variable recording the sum after rolling a die for $k$ turns, then:
    $$\Pb(Z_1 = 6) = \tfrac{1}{6}$$

    \begin{definition}[Conditional Probability]
        % Conditional probability is defined as the probability that an event occurs given the occurrence of another event:
        The probability of $A$ given $B$ is:
        $$\Pb(A\mid B) := \frac{\Pb(A\cap B)}{\Pb(B)}$$
    \end{definition}
    For example: $$\Pb(Z_3 = 18 \mid Z_1 = 6) = \tfrac{1}{36}$$

\end{frame}

\begin{frame}{Markov Chains}
\begin{definition}[Markov Chain]
An $\Ss$-valued discrete-time stochastic process $(Z_n)_{n \in \N}$ is said to be Markov if, for all $n\geq 1$, the probability distribution of $Z_{n+1}$ is determined by the state $Z_n$ of the process at time $n$ and does not depend on the past values of $Z_k$ for $k<n$. Here, $\Ss$ is a discrete state space, e.g. $\Ss = \Z$, $\Ss = \{0,1\}$, etc.
\end{definition}
\vspace{0.25 in}
In other words, for all $n\geq 1$ and all $i_0, i_1, \dots, i_n, j \in \Ss$ we have:
$$ \Pb(Z*{n+1} = j \mid Z_n = i_n, Z*{n-1} = i*{n-1}, \dots, Z_0 = i_0) = \Pb(Z*{n+1} = j \mid Z_n = i_n)$$
\end{frame}
\section{Why do we care?}
\begin{frame}{Examples of Markov Chains}
\begin{itemize}
\item \textbf{Weather} - the state of the weather right now can be predicted by yesterday's weather. We do not care too much about the weather, say, 3 years ago.
\item \textbf{PageRank algorithm} for rating the relevance of web-pages for a Google search.
\item \textbf{Text generation} - state space consists of different word sequences and next word(s) generated based on most recent word sequences.
\item Modeling in \textbf{gambling}, \textbf{finance}, \textbf{insurance claims}, \textbf{genetics}, etc.
\end{itemize}

\end{frame}
\section{How can we study them?}
\begin{frame}{First-step analysis (e.g. Ruin probabilities in gambling)}
Suppose you are gambling at a slot machine which costs \$1 to play and pays out \$2 with probability $0\leq w\leq 1$. Let $Z_i$ be how much money you have after playing for $i$ rounds.
\newline

    The \textbf{ruin probability} $\Pb(R\mid Z_0 = k)$ is defined as the probability that you lose all your money at some time, given that you start with \$k.
    \newline

    The key insight of \textbf{first-step analysis} is that ruin probability depends on the data of the starting point and
    not on the starting time, i.e.:
    $$\Pb(R \mid Z_1 = k \pm 1 \text{ and } Z_0 = k) = \Pb(R \mid Z_1 = k \pm 1)
    = \Pb(R \mid Z_0 = k \pm 1)$$

\end{frame}

\begin{frame}{First-step analysis (e.g. Ruin probabilities in gambling)}
From this, you can show that $\Pb(R \mid Z_0 = k)$ satisfies a recursive equation:
\begin{align*}
&\Pb(R \mid Z_0 = k) = w\cdot \Pb(R \mid Z_0 = k+1) + (1-w)\cdot\Pb(R \mid Z_0 = k-1)\\
&\Pb(R \mid Z_0 = 0) = 1\\
&\Pb(R \mid Z_0 = M) = 0
\end{align*}
where $M$ is the amount of money you would need to not play at all or stop playing. Solving this recursive system, you can show that:
$$\Pb(R \mid Z_0 = k) = \frac{\left(\frac{w}{1-w}\right)^{M-k}-1}{\left(\frac{w}{1-w}\right)^{M}-1} $$
\end{frame}

\begin{frame}{Transition Matrix (e.g. 2-state Markov Chain) }
Since Markov chains only depend on the previous state, you can encode how they evolve with a \textbf{transition matrix} $P$, which is made up of transition probabilities:
$$P_{i,j} := \Pb(Z_{n+1} = j \mid Z_n =i), i,j \in \Ss $$
Since these probabilities are independent of $n$, they are referred to as ``time homogeneous.''
\newline

    For example, consider the following two-state model, letting $Z_i$ be either $0$ or $1$ at time $i$:
    \begin{center}
            \includegraphics[scale=0.4]{2-STATE.png}
    \includegraphics[scale=0.4]{TRANSITION.png}
    \end{center}

\end{frame}

\begin{frame}{Transition Matrix (e.g. 2-state Markov Chain)}
Then, given a distribution at time $0$, denoted by $\pi_0$:
$$\pi_0 = \begin{bmatrix}
    \Pb(Z_0 = 0) & \Pb(Z_0 = 1)
    \end{bmatrix}$$
You can compute the distribution at time $1$ via:
$$ \pi_1 = \pi_0 P= \begin{bmatrix}
\Pb(Z_1 = 0) & \Pb(Z_1 = 1)
\end{bmatrix}$$
    In fact, the distribution at time $k$ is given by:
    $$\pi_k = \pi_0 P^k$$
$P^k$ is called the \textbf{Higher-Order transition matrix}.
\end{frame}

\begin{frame}{Transition Matrix (e.g. 2-state Markov Chain)}
For example, let $a = 0.2$ and $b = 0.4$. Then, you can find an explicit formula for $P^k$ by diagonalization:

    $$P^k = \frac{1}{0.6}\begin{bmatrix}
    0.4 + 0.2\cdot(0.4)^k & 0.2\cdot(1-(0.4)^k) \\
    0.4\cdot(1 - (0.4)^k) & 0.2 + 0.4\cdot(0.4)^k
    \end{bmatrix}, k \in \N
    $$

    Furthermore, given any initial distribution $\pi_0$, you can show that:
    $$\lim_{k\rightarrow \infty}\pi_0 P^k = \begin{bmatrix}\frac{2}{3}&\frac{1}{3}\end{bmatrix}$$
    This is called a \textbf{limiting distribution} and tells you for example that:
    $$\lim_{k\rightarrow\infty}\Pb(Z_k = 0\mid Z_0 = 0) =\lim_{k\rightarrow\infty}\Pb(Z_k = 0\mid Z_0 = 1) =  \frac{2}{3}$$

\end{frame}

\begin{frame}{References}

    \textbf{Book:} \textit{Understanding Markov Chains: Examples and Applications}, Second Edition, Nicolas Privault
    \newline


    \begin{center}
        Questions?
    \end{center}

\end{frame}

\end{document}
