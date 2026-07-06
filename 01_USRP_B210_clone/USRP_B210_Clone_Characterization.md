## Dual-RX coherence measurement with splitter

With the splitter connected, the measured amplitude mismatch was approximately:
CH0 − CH1 ≈ 0.5 dB
The measured relative phase difference was:
CH0 − CH1 ≈ 6.8°

CODE

TX_CHANS = [0, 1]
RX_CHANS = [0, 1]

tx0 = TX_AMPLITUDE * np.exp(1j * 2 * np.pi * TONE_FREQ * t)
tx1 = np.zeros(num_tx_samps, dtype=np.complex64)

tx_signal = np.vstack([
    tx0.astype(np.complex64),
    tx1.astype(np.complex64)
])

tx_args = uhd.usrp.StreamArgs("fc32", "sc16")
tx_args.channels = TX_CHANS
tx_streamer = usrp.get_tx_stream(tx_args)

rx_args = uhd.usrp.StreamArgs("fc32", "sc16")
rx_args.channels = RX_CHANS
rx_streamer = usrp.get_rx_stream(rx_args)

chunk = tx_signal[:, samples_sent:samples_sent + 4096]
sent = tx_streamer.send(chunk, tx_md, timeout=2.0)
