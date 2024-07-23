@def title = "Test"
@def tags = ["test", "general"]
@def isblog = true
@def showall = true
@def rss_pubdate = Date(2024, 7, 24)
@def rss_title = "Things to live by in 2024"
@def rss_description = "A letter to future to serve as a reminder to recenter myself throughout 2024."

# {{fill title}}
\toc

This past Christmas and new year season has been the most restful for me in recent memory: coming off the backside of the pandemic, making a career change, and moving across the country has occupied me the last few years. In this time, I've been trying to take better care of myself, seeing the doctor, etc. on things that have been bothering me but I've been putting off.

During this time, I've been reading a lot of essays that get shared on Hacker News that I've been finding quite insightful and inspiring. [One in particular](https://www.lesswrong.com/posts/uGDtroD26aLvHSoK2/dear-self-we-need-to-talk-about-ambition-1) was written in the form of a letter to their younger self with a lot of advice and introspection that I found extremely relatable, and partly inspired me to write this up; instead of a letter to my past self&mdash;I don't think I'm quite there yet&mdash;I thought I would write this up in the spirit of new year's resolutions, and serve to remind myself of more mindful times as the year inevitably picks up.

## Useless stuff
<!-- \begin{def}
Hi
\end{def} -->
@@def
\begin{def}{Interior and Closure}{Given a subset $S$ of a topological span $X$, the $\textit{interior}$ of $S$ is in $X$, denoted $\text{int}_X(S)$,
is defined as the union of all open sets contained in $S$, and the $\textit{closure of }S\textit{ in }X$, denoted $\text{cl}_X(S)$, 
is defined as the intersection of all closed sets containing $S$.}
\end{def}
@@

### More useless stuff

\begin{equation}
\begin{split}   a &=b+c\\
      &=e+f
\end{split}
\end{equation}

@@thm
WOW definition 1 is pretty cool!\sidenote{}{Sidenotes are cool!}   Ok[^1]
@@
[^1]: but footnotes are cooler!\sidenote{}{Agreed!}

I think we should really consider the works of \citep{baveja2024, bedi2024} because they're cool! Don't forget \citep{satinder2022} now.

\newcommand{\E}[1]{\mathbb E\left[#1\right]}


### Useless LaTeX
$$ R_n=\sum\limits_{t=n}^T \gamma^{t-n} r(s_t,a_t) $$
$$  \varphi(\E{X}) \le \E{\varphi(X)}.\label{equation blah} $$
\begin{eqnarray}
  \exp(i\pi)+1 &=& 0\\
  1+1 &=& 2
\end{eqnarray}

\begin{align}
  \exp(i\pi)+1 &= 0\\
  1+1 &= 2
\end{align}

but \eqref{equation blah} is fine.

## References

* \biblabel{baveja2024}{Baveja (2024)} **Baveja**, AAAI 2024.
* \biblabel{bedi2024}{Chakraborty et al. (2024)} **Chakraborty**, **Qiu**, **Bedi**, **Wang**, [MaxMin-RLHF: Towards Equitable Alignment of Large Language Models with Diverse Human Preferences](https://arxiv.org/pdf/2402.08925), ICML 2024.
* \biblabel{satinder2022}{Singh et al. (1996)} **Singh**, **Sutton**, [Reinforcement learning with replacing eligibility traces](https://link.springer.com/content/pdf/10.1023/A:1018012322525.pdf), Machine Learning 1996. 