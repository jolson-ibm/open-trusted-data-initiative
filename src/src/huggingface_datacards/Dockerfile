FROM python:3.12

COPY ./requirements.txt . 
RUN pip install -r requirements.txt

COPY ./src/get_hf_datacards.py .
CMD ["python3", "get_hf_datacards.py"]