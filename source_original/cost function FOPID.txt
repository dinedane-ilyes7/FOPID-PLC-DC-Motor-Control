function J = fopid_cost(x, motor_tf, omega_ref)
%FOPID_COST  PSO cost function for FOPID controller.
%
%  x = [Kp, Ki, Kd, lambda, mu]
%  Returns scalar cost J (lower = better).

Kp     = x(1);
Ki     = x(2);
Kd     = x(3);
lambda = x(4);
mu     = x(5);

V_max = 15;

%% -------------------------------------------------------
%  Parameter constraints
%% -------------------------------------------------------
if Kp<=0 || Ki<=0 || Kd<=0 || lambda<=0 || mu<=0
    J = 1e8; return;
end
if lambda > 1 || mu > 1
    J = 1e8; return;
end

try
    %% -------------------------------------------------------
    %  Build closed-loop system
    %% -------------------------------------------------------
    s        = fotf('s');
    motor_fo = fotf(motor_tf);

    C  = Kp + Ki/s^lambda + Kd*s^mu;
    CL = feedback(C * motor_fo, 1);

    if ~isstable(CL)
        J = 1e8; return;
    end

    %% -------------------------------------------------------
    %  Step simulation
    %% -------------------------------------------------------
    t    = (0:0.01:5)';
    y    = step(omega_ref * CL, t);
    y    = real(y(:));
    y(1) = 0;

    if any(isnan(y)) || any(isinf(y)) || max(abs(y)) > 15*omega_ref
        J = 1e8; return;
    end

    e = omega_ref - y;

    %% -------------------------------------------------------
    %  Oscillation guard
    %% -------------------------------------------------------
    zc = sum(diff(sign(e)) ~= 0);
    if zc > 15
        J = 1e8; return;
    end

    %% -------------------------------------------------------
    %  Control signal
    %% -------------------------------------------------------
    u     = real(lsim(C, e, t));
    u_sat = max(-V_max, min(V_max, u));

    %% -------------------------------------------------------
    %  Normalised metrics
    %% -------------------------------------------------------
    e_norm = e       / omega_ref;
    u_norm = u_sat   / V_max;

    J_itae   = trapz(t, t .* abs(e_norm));
    J_u      = trapz(t, u_norm.^2);
    du       = diff(u_norm);
    J_smooth = trapz(t(1:end-1), du.^2);

    overshoot = max(0, (max(y) - omega_ref) / omega_ref);
    sse       = abs(omega_ref - mean(y(end-10:end))) / omega_ref;
    sat_ratio = sum(abs(u) > V_max) / length(u);

    %% -------------------------------------------------------
    %  Cost function (balanced weights)
    %% -------------------------------------------------------
    J = 1.0   * J_itae       ...
      + 200.0  * overshoot^2 ...
      + 5   * sse^2        ...
      + 1   * J_u          ...
      + 1   * J_smooth     ...
      + 1   * sat_ratio;

    %% Extra penalties
    if sat_ratio > 0.1
        J = J + 200;
    end
    if sse > 0.01
        J = J + 50 * sse;
    end

    %% Gain regularisation (prevents extreme gains)
    J = J + 0.01*(Kp^2 + Ki^2 + Kd^2);

    %% Safety check
    if isnan(J) || isinf(J) || J < 0
        J = 1e8;
    end

catch
    J = 1e8;
end

end