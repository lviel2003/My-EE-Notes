# Fourier Series to Discrete Fourier Series

A Fourier series writes a periodic signal as a sum of sine and cosine waves.

The idea is simple. Instead of only describing the signal by its value in time, we describe it by how much of each frequency is inside the signal.

For a continuous periodic signal, the Fourier series is:

> [!eq1|centered]  
> $$  
> x(t)=a_0+\sum_{k=1}^{\infty}\left[a_k\cos\left(k\cdot2\pi f_0\cdot t\right)+b_k\sin\left(k\cdot2\pi f_0\cdot t\right)\right]  
> $$  
> ^continuous-fourier-series


> [!params]- Parameters  
> $$  
> \begin{aligned}  
> a_0&=\frac{1}{T_0}\int_0^{T_0}x(t)dt \quad \rightarrow \quad \text{Average value of the signal}\\[8pt]  
> a_k&=\frac{2}{T_0}\int_0^{T_0}x(t)\cos\left(k\cdot\frac{2\pi}{T_0}\cdot t\right)dt\\[8pt]  
> b_k&=\frac{2}{T_0}\int_0^{T_0}x(t)\sin\left(k\cdot\frac{2\pi}{T_0}\cdot t\right)dt\\[8pt]  
> T_0&=\text{Period of the signal }x(t)\\[8pt]  
> f_0&=\text{Fundamental frequency of the signal }x(t)\quad \rightarrow \quad f_0=\frac{1}{T_0}\\[8pt]  
> k&=\text{Harmonic index}  
> \end{aligned}  
> $$

The coefficient $a_0$ is the average value of the signal.

The coefficients $a_k$ and $b_k$ tell us how much cosine and sine we have at each harmonic frequency $kf_0$.

So:

1. $k=1$ is the fundamental frequency
    
2. $k=2$ is the second harmonic
    
3. $k=3$ is the third harmonic
    
4. etc.
    

The coefficient equations are basically doing this:

> Multiply the signal by the wave you want to detect, then average over one full period.

If that frequency is inside the signal, something remains.

If that frequency is not inside the signal, the positive and negative parts cancel.

---

## Amplitude and Phase Form

At each frequency, the sine and cosine terms can also be combined into one cosine with an amplitude and a phase shift.

For one harmonic:

> [!eq3|centered]  
> $$  
> a_k\cos(\theta)+b_k\sin(\theta)=c_k\cos(\theta-\phi_k)  
> $$

Therefore, the Fourier series can also be written as:

> [!eq1|centered]  
> $$  
> x(t)=\sum_{k=0}^{\infty}c_k\cos\left(k\cdot2\pi f_0\cdot t-\phi_k\right)  
> $$

> [!params]- Parameters  
> $$  
> \begin{aligned}  
> c_k&=\sqrt{a_k^2+b_k^2}\\[8pt]  
> \phi_k&=\operatorname{atan2}(b_k,a_k)  
> \end{aligned}  
> $$  
> The function $\operatorname{atan2}(b_k,a_k)$ is useful because it automatically gives the correct quadrant for the phase. This avoids manually checking the sign of $a_k$ and $b_k$.

This is the same information as the original Fourier series. We are just using amplitude and phase instead of separate sine and cosine coefficients.

---

# Discrete Fourier Series

Now we want to move from continuous time to discrete time. 

In continuous time, the signal exists for every value of $t$. 

In discrete time, we only keep the samples. This means the only time values we keep are $t=nT_s$. 

So the sampled signal is:
> [!eq3|centered]  
> $$  
> x[n]=x(nT_s)  
> $$

> [!params]- Parameters  
> $$  
> \begin{aligned}  
> T_s&=\text{Sampling period}\\[8pt]  
> f_s&=\text{Sampling frequency}\quad \rightarrow \quad f_s=\frac{1}{T_s}\\[8pt]  
> n&=\text{Sample index}\quad \rightarrow \quad n=0,1,2,3,\dots  
> \end{aligned}  
> $$

## Get the Discrete Fourier Series
1. Now start from the [[#^continuous-fourier-series|continuous Fourier series]] continuous Fourier series 
2. Since we only keep the samples, replace $t$ by $nT_s$:
$$  
x[n]=a_0+\sum_{k=1}^{\infty}\left[a_k\cos\left(k\cdot2\pi f_0\cdot nT_s\right)+b_k\sin\left(k\cdot2\pi f_0\cdot nT_s\right)\right]  
$$

3. Since $T_s=\frac{1}{f_s}$, we can replace $T_s$ by $\frac{1}{f_s}$:
$$  
x[n]=a_0+\sum_{k=1}^{\infty}\left[a_k\cos\left(\frac{k\cdot2\pi f_0}{f_s}n\right)+b_k\sin\left(\frac{k\cdot2\pi f_0}{f_s}n\right)\right]  
$$

4. Now we want to remove $f_0$ and $f_s$ from the equation. The reason is that once we are in discrete time, the most useful variable is the sample index $n$, not the continuous time $t$, so we prefer discrete variables instead of time domain variables. If one period contains $N$ samples ($N$ is integer), then:

> [!eq3|centered]  
> $$  
> N=\frac{T_0}{T_s} = \frac{f_s}{f_0}  \longrightarrow \frac{f_0}{f_s}=\frac{1}{N}  
> $$

5. Now replace $\frac{f_0}{f_s}$ by $\frac{1}{N}$:

> [!row|centered]
> 
> > [!eq2|centered]  
> > $$  
> > x[n]=a_0+\sum_{k=1}^{\infty}\left[a_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+b_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]  
> > $$
> 
> Sampled Fourier series before grouping repeated discrete frequencies  
> ^sampled-fourier-series-infinite

This equation makes sense, but there is still a problem, the summation is still infinite. A computer cannot compute an infinite number of terms. So now the question is:
> How can this infinite sum become finite after sampling?

> [!note]  
> You can always find the effective continuous frequency of a discrete-time sinusoid by converting it back to continuous time using the substitution $n=\frac{t}{T_s}$. 
> For example, if we have the discrete-time term $\cos(\pi n) = \cos\left(\pi\frac{t}{T_s}\right) = \cos(\pi f_s t) = \cos\left(2\pi\frac{f_s}{2}t\right)$. Therefore, $f=\frac{f_s}{2}$. However, this frequency is not unique, because frequencies separated by $f_s$ produce the same samples.

---

# Why Discrete Frequencies Repeat

In continuous time, every new value of $k$ creates a new harmonic frequency. In discrete time, this is not true anymore. Some different values of $k$ create exactly the same sampled waveform. This happens because the samples only exist at integer values of $n$.

The key idea is:
$$  
k \quad \text{and} \quad k+N \quad \text{create the same discrete-time frequency}  
$$

Let’s check it directly with cosine.
1. Start with the cosine term and replace $k$ by $k+N$:
   $$  
   \cos\left(\frac{2\pi(k+N)}{N}n\right)=\cos\left(\frac{2\pi k}{N}n+2\pi n\right)  
   $$
2. Since $n$ is an integer, $2\pi n$ is a full number of rotations, adding an integer number of rotations does not change the cosine.
$$  
\cos\left(\frac{2\pi k}{N}n+2\pi n\right)=\cos\left(\frac{2\pi k}{N}n\right)  \quad \longrightarrow \quad \cos\left(\frac{2\pi(k+N)}{N}n\right)=\cos\left(\frac{2\pi k}{N}n\right)  
$$


3. The same thing happens with sine:
   $$  
   \sin\left(\frac{2\pi(k+N)}{N}n\right)=\sin\left(\frac{2\pi k}{N}n\right)  
   $$

This means that discrete frequencies repeat every $N$. So instead of keeping infinitely many terms, we can group together all the terms that create the same sampled waveform.

For the rest of this note, we assume that $N$ is even.
> [!important]  
> We assume $N$ is even because then there is a special frequency at $k=\frac{N}{2}$. This is the Nyquist frequency.

---

# The Three Types of Terms

Since frequencies repeat every $N$, we need to group the repeated terms.

There are 3 types of terms:

1. DC type terms $(k=mN)$
    
2. Nyquist type terms  $(k=\frac{N}{2}+mN)$
    
3. Other terms $(1\leq k\leq\frac{N}{2}-1 )$
    

The goal is to group all continuous-time harmonics that become the same discrete-time frequency.

---

## DC Type Terms 
$(k=mN)$

The DC type terms happen when: $k=mN$, where $m$ is an integer.

1. Now plug $k=mN$ into the cosine term:
$$  
\cos\left(\frac{2\pi k}{N}n\right)\rightarrow\cos\left(\frac{2\pi mN}{N}n\right)=\cos(2\pi mn) = 1 \quad | \quad m,n \text{ are integers}
$$

2. Now plug $k=mN$ into the sine term:
$$  
\sin\left(\frac{2\pi k}{N}n\right)\rightarrow\sin\left(\frac{2\pi mN}{N}n\right)=\sin(2\pi mn) = 0 \quad | \quad m,n \text{ are integers}
$$

Therefore, every term where $k=mN$ behaves like DC. It does not create a changing waveform. It only adds to the average value of the signal.

If we observe only those terms inside the infinite sampled Fourier series:

$$  
x[n]=a_0+\cdots+\left[a_N\cancelto{1}{\cos(2\pi n)}+\cancelto{0}{b_N\sin(2\pi n)}\right]+\cdots+\left[a_{2N}\cancelto{1}{\cos(4\pi n)}+\cancelto{0}{b_{2N}\sin(4\pi n)}\right]+\cdots  = a_0+\cdots+a_N+\cdots+a_{2N}+\cdots  
$$

Therefore, all the DC type cosine coefficients can be grouped into one new coefficient:
> [!eq2|centered]  
> $$  
> \tilde{a}_0=a_0+\sum_{m=1}^{\infty}a_{mN}=a_0+a_N+a_{2N}+a_{3N}+\cdots  
> $$

This new coefficient $\tilde{a}_0$ is the discrete-time DC coefficient.

---

## Nyquist Type Terms 
$(k=\frac{N}{2}+mN)$

The Nyquist type terms happen when : $k=\frac{N}{2}+mN$. This is the frequency exactly halfway between $0$ and $f_s$ $\rightarrow f=\frac{f_s}{2}$. This is called the Nyquist frequency. After plugging $k=\frac{N}{2}+mN$ into the cosine term, the discrete-time component becomes $\cos(\pi n)$; then, since $n=\frac{t}{T_s}$ at the sampling instants, we get $\cos(\pi n)=\cos\left(\pi\frac{t}{T_s}\right)=\cos(\pi f_s t)=\cos\left(2\pi\frac{f_s}{2}t\right)$, which means this component corresponds to the frequency $\frac{f_s}{2}$.

1. Now plug $k=\frac{N}{2}+mN$ into the cosine term:
   $$  
   \cos\left(\frac{2\pi k}{N}n\right)\rightarrow\cos\left(\frac{2\pi}{N}\left(\frac{N}{2}+mN\right)n\right)=\cos(\pi n+2\pi mn) = \cos(\pi n)  
   $$

2. Now plug $k=\frac{N}{2}+mN$ into the sine term:
   $$  
   \sin\left(\frac{2\pi k}{N}n\right)\rightarrow\sin\left(\frac{2\pi}{N}\left(\frac{N}{2}+mN\right)n\right)=\sin(\pi n+2\pi mn) = \sin(\pi n) = 0
   $$

Therefore, all Nyquist type sine terms disappear. Only the cosine terms remain.

If we observe only those terms inside the infinite sampled Fourier series:
$$  
x[n]=\cdots+\left[a_{\frac{N}{2}}\cos(\pi n)+\cancelto{0}{b_{\frac{N}{2}}\sin(\pi n)}\right]+\cdots+\left[a_{\frac{3N}{2}}\cos(3\pi n)+\cancelto{0}{b_{\frac{3N}{2}}\sin(3\pi n)}\right]+\cdots  =  \cdots + (a_{\frac{N}{2}}+a_{\frac{3N}{2}}+a_{\frac{5N}{2}}+\cdots)\cos(\pi n) + \cdots
$$

Since $\cos(3\pi n)=\cos(\pi n)$ for integer $n$, and the same thing happens for $\cos(5\pi n)$, $\cos(7\pi n)$, etc., we can group all those cosine coefficients:

> [!eq2|centered]  
> $$  
> \tilde{a}_{N/2}=\sum_{m=0}^{\infty}a_{\frac{N}{2}+mN}=a_{\frac{N}{2}}+a_{\frac{3N}{2}}+a_{\frac{5N}{2}}+\cdots  
> $$

Therefore, the Nyquist part becomes:

> [!row|centered]
> 
> > [!eq3|centered]  
> > $$  
> > \tilde{a}_{N/2}\cos(\pi n)  
> > $$
>
> where $\omega=\pi \quad \rightarrow \quad f=\frac{f_s}{2}$

There is no $\tilde{b}_{N/2}$ term because: $\sin(\pi n)=0$. So the Nyquist component only has a cosine coefficient. 

---

## Other Terms 
$(1\leq k\leq\frac{N}{2}-1)$

The other terms are all values of $k$ that are not DC type terms and not Nyquist type terms. So the useful range is: $1\leq k\leq\frac{N}{2}-1$.

The reason we stop at $\frac{N}{2}-1$ is that $k=0$ is already the DC term, and $k=\frac{N}{2}$ is already the Nyquist term, so the "Other Terms" are literally all the other terms. 

Also, the terms above $\frac{N}{2}$ do not create new independent frequencies. They fold back and combine with the lower frequencies., which is why there are not values of $k$ above $\frac{N}{2}-1$.

For example, $k=N-1$ combines with $k=1$ as we can see with the cosine and sine terms:
$$\cos\left(\frac{(N-1)2\pi}{N}n\right)=\cos\left(2\pi n-\frac{2\pi}{N}n\right)=\cos\left(\frac{2\pi}{N}n\right)$$
$$\sin\left(\frac{(N-1)2\pi}{N}n\right)=\sin\left(2\pi n-\frac{2\pi}{N}n\right)=-\sin\left(\frac{2\pi}{N}n\right)$$

So $k=N-1$ is not a new frequency. It is the same cosine component as $k=1$, but with the opposite sine component.

That is why we only keep the normal frequency range: $1\leq k\leq\frac{N}{2}-1$. Everything above $\frac{N}{2}$ will be grouped with one of those lower-frequency terms. These terms are the normal sine and cosine terms.

Now we need to see exactly how those terms combine together.

1. The important pair is:
   $$k=r \quad \text{and} \quad k=N-r$$
   where $r$ is one of the other terms frequency indexes:
   $$1\leq r\leq\frac{N}{2}-1$$

2. First, look at the cosine for $k=N-r$ and compare it to the cosine for $k=r$:
   $$\cos\left(\frac{k\cdot2\pi}{N}n\right) = \cos\left(\frac{(N-r)2\pi}{N}n\right) = \cos\left(2\pi n-\frac{r2\pi}{N}n\right) = \cos\left(\frac{r2\pi}{N}n\right)$$
   So:
   $$\cos\left(\frac{(N-r)2\pi}{N}n\right)=\cos\left(\frac{r2\pi}{N}n\right)$$

3. Now look at the sine for $k=N-r$ and compare it to the sine for $k=r$:
   $$\sin\left(\frac{k\cdot2\pi}{N}n\right) = \sin\left(\frac{(N-r)2\pi}{N}n\right)=\sin\left(2\pi n-\frac{r2\pi}{N}n\right) = -\sin\left(\frac{r2\pi}{N}n\right)$$
   This time, the sine changes sign:
   $$\sin\left(2\pi n-\frac{r2\pi}{N}n\right)=-\sin\left(\frac{r2\pi}{N}n\right)$$

4. Therefore:
> [!row|centered]
> 
> > [!eq3|centered]  
> > $$\cos\left(\frac{(N-r)2\pi}{N}n\right)=\cos\left(\frac{r2\pi}{N}n\right)$$
> 
> > [!eq3|centered]  
> > $$\sin\left(\frac{(N-r)2\pi}{N}n\right)=-\sin\left(\frac{r2\pi}{N}n\right)$$

This means:

> The terms $r$ and $N-r$ have the same cosine component, but opposite sine components.

So their cosine coefficients add. But their sine coefficients subtract.

If we observe only those two terms inside the infinite sampled Fourier series:
$$x[n]=\cdots+\left[a_r\cos\left(\frac{r2\pi}{N}n\right)+b_r\sin\left(\frac{r2\pi}{N}n\right)\right]+\cdots+\left[a_{N-r}\cos\left(\frac{(N-r)2\pi}{N}n\right)+b_{N-r}\sin\left(\frac{(N-r)2\pi}{N}n\right)\right]+\cdots$$
Using the identities above:
> [!eq3|centered]  
> $$x[n]=\cdots+\left[(a_r+a_{N-r})\cos\left(\frac{r2\pi}{N}n\right)+(b_r-b_{N-r})\sin\left(\frac{r2\pi}{N}n\right)\right]+\cdots$$

So the term $k=N-r$ does not need its own separate frequency in the final equation, it gets absorbed into the lower frequency term $k=r$.

Now we also need to remember that frequencies repeat every $N$. So $k$, $k+N$, $k+2N$, etc. all create the same discrete-time frequency. This means that for each normal frequency $k$, we must group:
 $$k,\quad k+N,\quad k+2N,\quad \dots$$
and also the matching folded terms:
$$N-k,\quad N-k+N,\quad N-k+2N,\quad \dots$$
Therefore, for all normal terms, the grouped coefficients are:

> [!row|centered]
> 
> > [!eq2|centered]  
> > $$\tilde{a}_k=\sum_{m=0}^{\infty}\left(a_{k+mN}+a_{N-k+mN}\right)$$
> 
> > [!eq2|centered]  
> > $$\tilde{b}_k=\sum_{m=0}^{\infty}\left(b_{k+mN}-b_{N-k+mN}\right)$$

These equations say:
> All continuous-time harmonics that become the same discrete-time frequency are grouped into one discrete-time coefficient.



---

# New Discrete Fourier Series

Now we can replace the infinite sampled Fourier series with a finite one.
The finite real discrete Fourier series is:

> [!row|centered]
> 
> > [!eq2|centered]  
> > $$x[n]=\underbrace{\tilde{a}_0}_{\text{DC component}}+\underbrace{\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]}_{\text{Other terms sine/cosine components }(1\leq k\leq\frac{N}{2}-1)}+\underbrace{\tilde{a}_{N/2}\cos(\pi n)}_{\text{Nyquist component}}$$
> 
> Real Discrete Fourier Series with finite bounds  
> ^finite-real-dfs-equation

There is no sine term at DC and no sine term at Nyquist. That is why those two terms are special.

The total number of real coefficients is:
$$  
1+\left(\frac{N}{2}-1\right)+\left(\frac{N}{2}-1\right)+1=N  
$$
This makes sense because one period of $x[n]$ has $N$ samples. So we need $N$ independent numbers to describe it.

---

# Finding the Finite Coefficients From the Samples

The previous section explains why the infinite sum becomes finite, but we still have one issue. The grouped coefficients $\tilde{a}_0$, $\tilde{a}_k$, $\tilde{b}_k$, and $\tilde{a}_{N/2}$ were written as infinite sums of the continuous Fourier coefficients. That explains the theory, but it is not useful for computation.

In practice, we usually already have the samples $x[n]$. So now the goal is:
> Find the finite coefficients directly from the samples $x[n]$.

To do so, the idea is always the same:
> Multiply by the wave we want to isolate, then sum over one full period.

If we multiply by the correct wave, that coefficient survives.
If we multiply by the wrong wave, the result cancels to zero.

This works because sine and cosine waves are orthogonal over one period.
In simple words:
> Different sine and cosine waves cancel each other over one full period. (See next section)

---

## Useful Cancellation Facts

For normal frequencies where $1\leq k\leq\frac{N}{2}-1$:

> [!row|centered]
> 
> > [!eq3|centered]  
> > $$  
> > \sum_{n=0}^{N-1}\cos\left(\frac{2\pi k}{N}n\right)=0  
> > $$
> 
> > [!eq3|centered]  
> > $$  
> > \sum_{n=0}^{N-1}\sin\left(\frac{2\pi k}{N}n\right)=0  
> > $$

This means normal sine and cosine waves have zero average over one PERIOD.

The Nyquist cosine also has zero average:

> [!eq3|centered]  
> $$  
> \sum_{n=0}^{N-1}\cos(\pi n)=0  
> $$

This happens because:
$$\cos(\pi n)=(-1)^n = 1-1+1-1+\dots =0 \quad | \quad n\text{ is even, so it cancel to 0}$$
For products, the useful facts are:

> [!row|centered]
> 
> > [!eq3|centered]  
> > $$  
> > \sum_{n=0}^{N-1}\cos\left(\frac{2\pi k}{N}n\right)\cos\left(\frac{2\pi p}{N}n\right)=0,\quad p\neq k  
> > $$
> 
> > [!eq3|centered]  
> > $$  
> > \sum_{n=0}^{N-1}\cos\left(\frac{2\pi k}{N}n\right)\cos\left(\frac{2\pi p}{N}n\right)=\frac{N}{2},\quad p=k  
> > $$

> [!row|centered]
> 
> > [!eq3|centered]  
> > $$  
> > \sum_{n=0}^{N-1}\sin\left(\frac{2\pi k}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)=0,\quad p\neq k  
> > $$
> 
> > [!eq3|centered]  
> > $$  
> > \sum_{n=0}^{N-1}\sin\left(\frac{2\pi k}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)=\frac{N}{2},\quad p=k  
> > $$

> [!row|centered]
> 
> > [!eq3|centered]  
> > $$  
> > \sum_{n=0}^{N-1}\cos\left(\frac{2\pi k}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)=0
> > $$
> 
> Cosine times sine always cancels.

> [!row|centered]
> 
> > [!eq3|centered]  
> > $$  
> > \sum_{n=0}^{N-1}\cos^2(\pi n)=\sum_{n=0}^{N-1}1 = N  
> > $$
> 
> The Nyquist cosine is special.



---

> [!hypothesis|*]- Proof for the Finite Coefficients of the Real DFS  
> For each coefficient, the logic is always the same.
> 
> 1. Start from the [[#^finite-real-dfs-equation|finite real DFS equation]]
>     
> 2. Multiply by the wave connected to the coefficient we want
>     
> 3. Sum over one full period
>     
> 4. Everything cancels except the coefficient we want
>     
> 
> ### The DC Coefficient $\tilde{a}_0$
> 
> To find the DC coefficient, we sum both sides of the [[#^finite-real-dfs-equation|finite real DFS equation]] from $n=0$ to $N-1$.
> 
> Start with:
> 
> $$  
> x[n]=\tilde{a}_0+\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]+\tilde{a}_{N/2}\cos(\pi n)  
> $$
> 
> Sum both sides:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]=\sum_{n=0}^{N-1}\left(\tilde{a}_0+\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]+\tilde{a}_{N/2}\cos(\pi n)\right)  
> $$
> 
> Separate the terms:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]=\sum_{n=0}^{N-1}\tilde{a}_0+\sum_{k=1}^{N/2-1}\sum_{n=0}^{N-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]+\sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos(\pi n)  
> $$
> 
> Now cancel what averages to zero:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]=\sum_{n=0}^{N-1}\tilde{a}_0+\sum_{k=1}^{N/2-1}\cancelto{0}{\sum_{n=0}^{N-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]}+\cancelto{0}{\sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos(\pi n)}  
> $$
> 
> Only the DC term remains:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]=N\tilde{a}_0  
> $$
> 
> Therefore:
> 
> $$  
> \tilde{a}_0=\frac{1}{N}\sum_{n=0}^{N-1}x[n]  
> $$
> 
> This makes sense because the DC coefficient is just the average value of the samples.
> 
> ---
> 
> ### The Cosine Coefficients $\tilde{a}_k$
> 
> Now we want to find one other terms cosine coefficient.
> 
> To avoid confusing the coefficient index with the summation index, call the coefficient we want $\tilde{a}_p$.
> 
> The value $p$ is one specific frequency index:
> 
> $$  
> 1\leq p\leq\frac{N}{2}-1  
> $$
> 
> To isolate $\tilde{a}_p$, multiply both sides by the matching cosine:
> 
> $$  
> \cos\left(\frac{2\pi p}{N}n\right)  
> $$
> 
> Start with:
> 
> $$  
> x[n]=\tilde{a}_0+\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]+\tilde{a}_{N/2}\cos(\pi n)  
> $$
> 
> Multiply both sides by $\cos\left(\frac{2\pi p}{N}n\right)$:
> 
> $$  
> x[n]\cos\left(\frac{2\pi p}{N}n\right)=\tilde{a}_0\cos\left(\frac{2\pi p}{N}n\right)+\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)\cos\left(\frac{2\pi p}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\cos\left(\frac{2\pi p}{N}n\right)\right]+\tilde{a}_{N/2}\cos(\pi n)\cos\left(\frac{2\pi p}{N}n\right)  
> $$
> 
> Now sum both sides:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\cos\left(\frac{2\pi p}{N}n\right)=\sum_{n=0}^{N-1}\tilde{a}_0\cos\left(\frac{2\pi p}{N}n\right)+\sum_{k=1}^{N/2-1}\sum_{n=0}^{N-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)\cos\left(\frac{2\pi p}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\cos\left(\frac{2\pi p}{N}n\right)\right]+\sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos(\pi n)\cos\left(\frac{2\pi p}{N}n\right)  
> $$
> 
> Now cancel everything that averages to zero:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\cos\left(\frac{2\pi p}{N}n\right)=\cancelto{0}{\sum_{n=0}^{N-1}\tilde{a}_0\cos\left(\frac{2\pi p}{N}n\right)}+\sum_{k=1}^{N/2-1}\sum_{n=0}^{N-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)\cos\left(\frac{2\pi p}{N}n\right)+\cancelto{0}{\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\cos\left(\frac{2\pi p}{N}n\right)}\right]+\cancelto{0}{\sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos(\pi n)\cos\left(\frac{2\pi p}{N}n\right)}  
> $$
> 
> Only the cosine terms can survive.
> 
> But even the cosine terms cancel unless $k=p$:
> 
> $$  
> \sum_{n=0}^{N-1}\cos\left(\frac{k\cdot2\pi}{N}n\right)\cos\left(\frac{2\pi p}{N}n\right)=0,\quad k\neq p  
> $$
> 
> When $k=p$, the cosine is multiplied by itself:
> 
> $$  
> \sum_{n=0}^{N-1}\tilde{a}_p\cos^2\left(\frac{2\pi p}{N}n\right)=\tilde{a}_p\frac{N}{2}  
> $$
> 
> So after all cancellations:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\cos\left(\frac{2\pi p}{N}n\right)=\tilde{a}_p\frac{N}{2}  
> $$
> 
> Isolate $\tilde{a}_p$:
> 
> $$  
> \tilde{a}_p=\frac{2}{N}\sum_{n=0}^{N-1}x[n]\cos\left(\frac{2\pi p}{N}n\right)  
> $$
> 
> Since $p$ was just the frequency index we wanted to isolate, rename it back to $k$:
> 
> $$  
> \tilde{a}_k=\frac{2}{N}\sum_{n=0}^{N-1}x[n]\cos\left(\frac{2\pi k}{N}n\right),\quad 1\leq k\leq\frac{N}{2}-1  
> $$
> 
> ---
> 
> ### The Sine Coefficients $\tilde{b}_k$
> 
> Now we want to find one other terms sine coefficient.
> 
> Again, call the coefficient we want $\tilde{b}_p$.
> 
> The value $p$ is one specific frequency index:
> 
> $$  
> 1\leq p\leq\frac{N}{2}-1  
> $$
> 
> To isolate $\tilde{b}_p$, multiply both sides by the matching sine:
> 
> $$  
> \sin\left(\frac{2\pi p}{N}n\right)  
> $$
> 
> Start with:
> 
> $$  
> x[n]=\tilde{a}_0+\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]+\tilde{a}_{N/2}\cos(\pi n)  
> $$
> 
> Multiply both sides by $\sin\left(\frac{2\pi p}{N}n\right)$:
> 
> $$  
> x[n]\sin\left(\frac{2\pi p}{N}n\right)=\tilde{a}_0\sin\left(\frac{2\pi p}{N}n\right)+\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)\right]+\tilde{a}_{N/2}\cos(\pi n)\sin\left(\frac{2\pi p}{N}n\right)  
> $$
> 
> Now sum both sides:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\sin\left(\frac{2\pi p}{N}n\right)=\sum_{n=0}^{N-1}\tilde{a}_0\sin\left(\frac{2\pi p}{N}n\right)+\sum_{k=1}^{N/2-1}\sum_{n=0}^{N-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)\right]+\sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos(\pi n)\sin\left(\frac{2\pi p}{N}n\right)  
> $$
> 
> Now cancel everything that averages to zero:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\sin\left(\frac{2\pi p}{N}n\right)=\cancelto{0}{\sum_{n=0}^{N-1}\tilde{a}_0\sin\left(\frac{2\pi p}{N}n\right)}+\sum_{k=1}^{N/2-1}\sum_{n=0}^{N-1}\left[\cancelto{0}{\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)}+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)\right]+\cancelto{0}{\sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos(\pi n)\sin\left(\frac{2\pi p}{N}n\right)}  
> $$
> 
> Only the sine terms can survive.
> 
> But even the sine terms cancel unless $k=p$:
> 
> $$  
> \sum_{n=0}^{N-1}\sin\left(\frac{k\cdot2\pi}{N}n\right)\sin\left(\frac{2\pi p}{N}n\right)=0,\quad k\neq p  
> $$
> 
> When $k=p$, the sine is multiplied by itself:
> 
> $$  
> \sum_{n=0}^{N-1}\tilde{b}_p\sin^2\left(\frac{2\pi p}{N}n\right)=\tilde{b}_p\frac{N}{2}  
> $$
> 
> So after all cancellations:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\sin\left(\frac{2\pi p}{N}n\right)=\tilde{b}_p\frac{N}{2}  
> $$
> 
> Isolate $\tilde{b}_p$:
> 
> $$  
> \tilde{b}_p=\frac{2}{N}\sum_{n=0}^{N-1}x[n]\sin\left(\frac{2\pi p}{N}n\right)  
> $$
> 
> Since $p$ was just the frequency index we wanted to isolate, rename it back to $k$:
> 
> $$  
> \tilde{b}_k=\frac{2}{N}\sum_{n=0}^{N-1}x[n]\sin\left(\frac{2\pi k}{N}n\right),\quad 1\leq k\leq\frac{N}{2}-1  
> $$
> 
> ---
> 
> ### The Nyquist Coefficient $\tilde{a}_{N/2}$
> 
> Now we want to find the Nyquist coefficient.
> 
> The Nyquist basis function is:
> 
> $$  
> \cos(\pi n)  
> $$
> 
> So we multiply both sides by $\cos(\pi n)$.
> 
> Start with:
> 
> $$  
> x[n]=\tilde{a}_0+\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]+\tilde{a}_{N/2}\cos(\pi n)  
> $$
> 
> Multiply both sides by $\cos(\pi n)$:
> 
> $$  
> x[n]\cos(\pi n)=\tilde{a}_0\cos(\pi n)+\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)\cos(\pi n)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\cos(\pi n)\right]+\tilde{a}_{N/2}\cos^2(\pi n)  
> $$
> 
> Now sum both sides:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\cos(\pi n)=\sum_{n=0}^{N-1}\tilde{a}_0\cos(\pi n)+\sum_{k=1}^{N/2-1}\sum_{n=0}^{N-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)\cos(\pi n)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\cos(\pi n)\right]+\sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos^2(\pi n)  
> $$
> 
> Now cancel everything that averages to zero:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\cos(\pi n)=\cancelto{0}{\sum_{n=0}^{N-1}\tilde{a}_0\cos(\pi n)}+\sum_{k=1}^{N/2-1}\sum_{n=0}^{N-1}\left[\cancelto{0}{\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)\cos(\pi n)}+\cancelto{0}{\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\cos(\pi n)}\right]+\sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos^2(\pi n)  
> $$
> 
> Only the Nyquist term survives:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\cos(\pi n)=\sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos^2(\pi n)  
> $$
> 
> Since:
> 
> $$  
> \cos(\pi n)=(-1)^n  
> $$
> 
> we have:
> 
> $$  
> \cos^2(\pi n)=1  
> $$
> 
> Therefore:
> 
> $$  
> \sum_{n=0}^{N-1}\tilde{a}_{N/2}\cos^2(\pi n)=\tilde{a}_{N/2}\sum_{n=0}^{N-1}1=N\tilde{a}_{N/2}  
> $$
> 
> So:
> 
> $$  
> \sum_{n=0}^{N-1}x[n]\cos(\pi n)=N\tilde{a}_{N/2}  
> $$
> 
> Isolate $\tilde{a}_{N/2}$:
> 
> $$  
> \tilde{a}_{N/2}=\frac{1}{N}\sum_{n=0}^{N-1}x[n]\cos(\pi n)  
> $$
> 
> This coefficient has $\frac{1}{N}$ instead of $\frac{2}{N}$ because the Nyquist cosine squared is always $1$, not something with average $\frac{1}{2}$.
> 
> ^dfs-coefficient-derivation

From the [[#^dfs-coefficient-derivation|DFS coefficient derivation]], the finite equations for the coefficients are:

> [!row|centered]
> 
> > [!eq1|centered]  
> > $$  
> > \tilde{a}_0=\frac{1}{N}\sum_{n=0}^{N-1}x[n]  
> > $$
> 
> > [!eq1|centered]  
> > $$  
> > \tilde{a}_k=\frac{2}{N}\sum_{n=0}^{N-1}x[n]\cos\left(\frac{2\pi k}{N}n\right)  
> > $$

> [!row|centered]
> 
> > [!eq1|centered]  
> > $$  
> > \tilde{b}_k=\frac{2}{N}\sum_{n=0}^{N-1}x[n]\sin\left(\frac{2\pi k}{N}n\right)  
> > $$
> 
> > [!eq1|centered]  
> > $$  
> > \tilde{a}_{N/2}=\frac{1}{N}\sum_{n=0}^{N-1}x[n]\cos(\pi n)  
> > $$

>[!params]- Valid ranges  
>$$\begin{aligned}1\leq k\leq\frac{N}{2}-1 \quad &\rightarrow \quad \text{Normal sine/cosine terms}\\[8pt] k=\frac{N}{2} \quad &\rightarrow \quad \text{Nyquist term}\\[8pt] N&=\text{Even number of samples per period}\end{aligned}$$  
>The normal terms stop at $k=\frac{N}{2}-1$ because $k=\frac{N}{2}$ is already the Nyquist frequency. Values above $\frac{N}{2}$ do not create new frequencies. They fold back into the lower half. For example, $k=N-1$ folds back into $k=1$ because $\cos\left(\frac{(N-1)2\pi}{N}n\right)=\cos\left(\frac{2\pi}{N}n\right)$, but $\sin\left(\frac{(N-1)2\pi}{N}n\right)=-\sin\left(\frac{2\pi}{N}n\right)$. So the higher-frequency index does not need its own separate term; it gets grouped with the lower-frequency coefficient.



So the final real DFS equation is:

> [!row|centered]
> 
> > [!eq1|centered]  
> > $$  
> > x[n]=\tilde{a}_0+\sum_{k=1}^{N/2-1}\left[\tilde{a}_k\cos\left(\frac{k\cdot2\pi}{N}n\right)+\tilde{b}_k\sin\left(\frac{k\cdot2\pi}{N}n\right)\right]+\tilde{a}_{N/2}\cos(\pi n)  
> > $$
> 
> Final real DFS equation  
> ^final-real-dfs-equation

---

# Important Note About the FFT

The equations above are for the real discrete Fourier series.

The FFT is not a different transform.

The FFT is just a fast algorithm used to compute the DFT.

So the logic is:

1. Fourier series decomposes a periodic signal into frequencies.
    
2. Sampling gives the discrete signal $x[n]$.
    
3. Discrete frequencies repeat every $N$.
    
4. Because of that repetition, the infinite sampled Fourier series becomes finite.
    
5. The DFS / DFT gives the finite frequency coefficients.
    
6. The FFT computes those coefficients faster.
    

So:

> [!row|centered]
> 
> > [!eq3|centered]  
> > $$  
> > \text{DFT}=\text{the transform}  
> > $$
> 
> > [!eq3|centered]  
> > $$  
> > \text{FFT}=\text{the fast algorithm}  
> > $$

> [!important]  
> In this note, $N$ is assumed to be even. This is why the special Nyquist term $\tilde{a}_{N/2}\cos(\pi n)\;$  exists. If $N$ is odd, there is no exact $N/2$ term.




