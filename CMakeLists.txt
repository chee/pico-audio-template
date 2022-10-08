cmake_minimum_required(VERSION 3.23)
include(pico/sdk/external/pico_sdk_import.cmake)
include(pico/extras/external/pico_extras_import.cmake)
project(sound_program C CXX)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
pico_sdk_init()

if(TARGET pico_audio_i2s)
	add_executable(sound_program_i2s
		main.c
	)

	target_link_libraries(sound_program_i2s PRIVATE
		pico_stdlib
		pico_audio_i2s
	)

	target_compile_definitions(sound_program_i2s PRIVATE

		# compile time configuration of I2S
		PICO_AUDIO_I2S_MONO_INPUT=1
		USE_AUDIO_I2S=1

		# PICO_AUDIO_I2S_DATA_PIN=22
		# PICO_AUDIO_I2S_CLOCK_PIN_BASE=23
		# PICO_DEFAULT_UART=0
		# PICO_DEFAULT_UART_TX_PIN=28
		# PICO_DEFAULT_UART_RX_PIN=29
	)

	# create map/bin/hex file etc.
	pico_add_extra_outputs(sound_program_i2s)
endif()

# only build PWM example if library is available
if(TARGET pico_audio_pwm)
	add_executable(sound_program_pwm
		main.c
	)

	target_link_libraries(sound_program_pwm PRIVATE
		pico_stdlib
		pico_audio_pwm
	)

	target_compile_definitions(sound_program_pwm PRIVATE

		# define for our example code
		USE_AUDIO_PWM=1
	)

	# create map/bin/hex file etc.
	pico_add_extra_outputs(sound_program_pwm)
endif()

# only build S/PDIF example if library is available
if(TARGET pico_audio_spdif)
	add_executable(sound_program_spdif
		main.c
	)

	target_link_libraries(sound_program_spdif PRIVATE
		pico_stdlib
		pico_audio_spdif
	)

	target_compile_definitions(sound_program_spdif PRIVATE

		# compile time configuration of S/PDIF
		PICO_AUDIO_SPDIF_MONO_INPUT=1

		# define for our example code
		USE_AUDIO_SPDIF=1
	)

	# create map/bin/hex file etc.
	pico_add_extra_outputs(sound_program_spdif)
endif()
