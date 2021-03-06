% This function is made by Ahmed ElTahan

%{
        This function is intended to estimate the parameters of a dynamic
        system of unknown parameters using the Recursive Modified Extended Least Squares Method (RMLS).
        After an experiment, we get the inputs, the outputs of the system. 
        The experiment is operated with sample time Ts seconds. 
                                    
        The model is given by           A(z) y(t) = B(z)sys u(t) + C(z) eps(t) 
        which can be written in
                                                                     z^(-d) B(z)                C(z)
                                                         y(t) = ------------------- u  + ------------ e = L*u + M*e
                                                                         A(z)                       A(z)

    where:
    -- y : output of the system.
    -- u : control action (input to the system).
    -- e : color guassian noise (noise with non zero mean).
    -- Asys = 1 + a_1 z^-1 + a_2 z^-2 + ... + a_na z^(-na). [denominator polynomail]
    -- Bsys = b_0 + b_1 z^-1 + b_2 z^-2 + ... + b_nb z^(-nb). [numerator polynomail]
    -- C = 1 + c_1 z^-1 + c_2 z^-2 + ... + c_nc z^(-nc). [noise characteristics]
    -- d : delay in the system.
    A and C are monic polynomials. (in output estimation of the stochastic
    system as C is monic, we add e(t) to the estimation i.e. not starting from c1*e(t-1))


    Function inputs
    u : input to the system in column vector form
    y : input of the system in column vector form
    na : order of the denominator polynomail
    nb : order of the numerator polynomail
    nc : order of the characteristics of the noise (usually <=2 for max)
    d : number represents the delay between the input and the output
    
    Function Output
    Theta_final : final estimated parameters.
    Gz_estm : pulse (discrete) transfer function of the estimated parameters
    1 figure for the history of the parameters that are being estimated
    2 figure to validate the estimated parameters on the given output
    using the instantaneous estimated parameters.
    3 figure to plot the input versus time.

        Note: the noise added shall not to be with a magnitude close to the
        system output, it should be smaller, this is in simulation such as
        here or the algorithm will go crazy that can't distinguish between
        the main and the noisy signal (This can be measured in practical 
        case finding noise to signal ratio).

    An example is added to illustrate how to use the funcrtion
