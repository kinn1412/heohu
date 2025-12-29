# Layered Source Inventory

## File Inventory & API Map
| LAYER | MODULE | FILES | PUBLIC APIs |
| --- | --- | --- | --- |
| DRIVER | ADC | `/home/runner/work/heohu/heohu/main/adc.c`<br>`/home/runner/work/heohu/heohu/main/adc.h` | `void ADC_Init(void);` |
| DRIVER | GPIO | `/home/runner/work/heohu/heohu/main/gpio.c`<br>`/home/runner/work/heohu/heohu/main/gpio.h` | `void GPIO_Init();` |
| DRIVER | TIMER | `/home/runner/work/heohu/heohu/main/timer.c`<br>`/home/runner/work/heohu/heohu/main/timer.h` | `void Timer_Init();`<br>`bool Reading_ECG_Sensor(gptimer_handle_t timer, const gptimer_alarm_event_data_t *edata, void *user_ctx);` |
| DRIVER | WIFI_STA | `/home/runner/work/heohu/heohu/main/wifi_sta.c`<br>`/home/runner/work/heohu/heohu/main/wifi_sta.h` | `void wifi_init_sta(void);` |
| SERVICE | TB_HTTP | `/home/runner/work/heohu/heohu/main/tb_http.c`<br>`/home/runner/work/heohu/heohu/main/tb_http.h` | `void tb_http_init(void);`<br>`esp_err_t tb_http_send_json(const char *json_payload);` |
| SERVICE | QUEUE | `/home/runner/work/heohu/heohu/main/Queue.c`<br>`/home/runner/work/heohu/heohu/main/Queue.h` | `void Queue_Init(QueueHandle_t *queue, uint32_t QueueLength, uint32_t ItemSize);` |
| SERVICE | SEMAPHORE | `/home/runner/work/heohu/heohu/main/semaphore.c`<br>`/home/runner/work/heohu/heohu/main/semaphore.h` | `void Semaphore_Init(SemaphoreHandle_t *sem);` |
| SERVICE | HPF | `/home/runner/work/heohu/heohu/main/HPF.c`<br>`/home/runner/work/heohu/heohu/main/HPF.h` | `HPFType *HPF_create(void);`<br>`void HPF_destroy(HPFType *pObject);`<br>`void HPF_init(HPFType * pThis);`<br>`void HPF_reset(HPFType * pThis);`<br>`int HPF_filterBlock(HPFType * pThis, float * pInput, float * pOutput, unsigned int count);`<br>`void HPF_filterBiquad(HPF_executionState * pExecState);` |
| SERVICE | LPF | `/home/runner/work/heohu/heohu/main/LPF.c`<br>`/home/runner/work/heohu/heohu/main/LPF.h` | `LPFType *LPF_create(void);`<br>`void LPF_destroy(LPFType *pObject);`<br>`void LPF_init(LPFType * pThis);`<br>`void LPF_reset(LPFType * pThis);`<br>`int LPF_filterBlock(LPFType * pThis, float * pInput, float * pOutput, unsigned int count);`<br>`void LPF_filterBiquad(LPF_executionState * pExecState);` |
| SERVICE | SBF | `/home/runner/work/heohu/heohu/main/SBF.c`<br>`/home/runner/work/heohu/heohu/main/SBF.h` | `SBFType *SBF_create(void);`<br>`void SBF_destroy(SBFType *pObject);`<br>`void SBF_init(SBFType * pThis);`<br>`void SBF_reset(SBFType * pThis);`<br>`int SBF_filterBlock(SBFType * pThis, float * pInput, float * pOutput, unsigned int count);`<br>`void SBF_filterBiquad(SBF_executionState * pExecState);` |
| SERVICE | PAN_TOMPKINS | `/home/runner/work/heohu/heohu/main/PanTompkins.c`<br>`/home/runner/work/heohu/heohu/main/PanTompkins.h` | `void PT_init(void);`<br>`int16_t PT_StateMachine(int16_t datum);`<br>`void LearningPhase1(uint16_t *pkI, int16_t *pkBP);`<br>`void LPFilter(int16_t *val);`<br>`void HPFilter(void);`<br>`void DerivFilter(void);`<br>`void SQRFilter(void);`<br>`void MVAFilter(void);`<br>`uint16_t PeakDtcI(void);`<br>`void PeakDtcDR(int16_t DR_sample);`<br>`void PeakDtcBP(int16_t DR_sample);`<br>`void UpdateRR(int16_t qrs);`<br>`void UpdateThI(uint16_t *PEAKI, int8_t NOISE_F);`<br>`void UpdateThF(int16_t *PEAKF, int8_t NOISE_F);`<br>`int16_t PT_get_LPFilter_output(void);`<br>`int16_t PT_get_HPFilter_output(void);`<br>`int16_t PT_get_DRFilter_output(void);`<br>`uint16_t PT_get_MVFilter_output(void);`<br>`uint16_t PT_get_SQRFilter_output(void);`<br>`int16_t PT_get_ShortTimeHR_output(int16_t Fs);`<br>`int16_t PT_get_LongTimeHR_output(int16_t Fs);`<br>`uint16_t PT_get_ThI1_output(void);`<br>`int16_t PT_get_ThF1_output(void);`<br>`uint16_t PT_get_SKPI_output(void);`<br>`uint16_t PT_get_NPKI_output(void);`<br>`int16_t PT_get_SPKF_output(void);`<br>`int16_t PT_get_NPKF_output(void);`<br>`int16_t PT_get_HRState_output(void);` |
| APPLICATION | MAIN | `/home/runner/work/heohu/heohu/main/main.c`<br>`/home/runner/work/heohu/heohu/main/main.h` | `int16_t filtering(int data);`<br>`void build_json_packet(char *buffer, size_t max_len, const ecg_msg_t *msg);`<br>`int16_t downsampling(int16_t data);`<br>`void ECG_Reading_Task(void *pvParameters);`<br>`void HTTP_Transmission_Task(void *pvParameters);`<br>`void app_main(void);` |

## Notes / Justification
- **ADC (DRIVER)**: Directly configures and reads ESP32 ADC peripheral through HAL types and oneshot API; no application logic beyond primitive sampling setup.
- **GPIO (DRIVER)**: Only sets up GPIO pins as inputs for lead-off detection; purely hardware configuration.
- **TIMER (DRIVER)**: Creates and configures GPTimer and ISR callback to signal sampling; hardware timing control without higher-level flow.
- **WIFI_STA (DRIVER)**: Initializes ESP Wi‑Fi station interface and handles connection events; interacts directly with networking peripheral stack, no business logic.
- **TB_HTTP (SERVICE)**: Provides reusable HTTP client init/send routines over ThingsBoard; middleware that can serve multiple app flows.
- **QUEUE (SERVICE)**: Thin wrapper around FreeRTOS queue creation for reuse; no hardware or app-specific behavior.
- **SEMAPHORE (SERVICE)**: Creates binary semaphore wrapper for synchronization; generic utility independent of app flow.
- **HPF / LPF / SBF (SERVICE)**: Signal-processing filters implemented as reusable DSP blocks; operate on input buffers without app orchestration.
- **PAN_TOMPKINS (SERVICE)**: Heartbeat detection algorithm/state machine encapsulated for reuse; no direct hardware control or main flow management.
- **MAIN (APPLICATION)**: Entry-point orchestrating Wi‑Fi, peripherals, filters, tasks, and data pipeline; contains application tasks, JSON building, and scheduling logic tying services together.
