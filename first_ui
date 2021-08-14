import os
import tkinter as tk
import logging
import subprocess
import sys
from tkinter import ttk


def run_nordic_command(sub_cmd):
    path = '/Users/yanfeng/Desktop/nrf_shell'
    cmd = os.path.join(path, "nrf_shell.py")
    final_cmd = ["python2", cmd, "--uart", "--commands", sub_cmd]
    p = subprocess.Popen(final_cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE, cwd=path)
    p.wait()
    result = p.stdout.read().decode("utf-8").strip()
    return result


def format_git_result(result):
    result_list = result.split('\n')
    version_result = result_list[3].replace("tag: ", "")
    version = (version_result.split('-'))[0]
    fw_version = version[1:10]
    return fw_version


def format_self_test_result(result):
    result_list = result.split('\n')
    self_result = result_list[-1].replace("test_passed: ", "")
    return self_result


def cmd_with_ui(tree):
    result = run_nordic_command('git')
    version = format_git_result(result)
    if version == 'v4.4.0025':
        fail_pass = "PASS"
    else:
        fail_pass = "FAIL"
    final_str_git = 'git', version, fail_pass
    final_str = list()
    final_str.append(final_str_git)

    result = run_nordic_command('self_test')
    self_result = format_self_test_result(result)
    if self_result == 'true':
        fail_pass = "PASS"
    else:
        fail_pass = "FAIL"
    final_str_self = 'self_test', self_result, fail_pass
    final_str.append(final_str_self)
    print(final_str)
    for i in range(len(final_str)):
        set_str_color = final_str[i]
        print("set_str_color:", set_str_color)
        if set_str_color[2] == "PASS":
            tree.insert('', 'end', values=set_str_color, tag=('P',))
            tree.tag_configure('P', background='green')
        else:
            tree.insert('', 'end', values=set_str_color, tag=('F',))
            tree.tag_configure('F', background='red')


def my_ui():
    root = tk.Tk()

    HEIGHT = 800
    WIDTH = 800

    canvas = tk.Canvas(root, height=HEIGHT, width=WIDTH)
    canvas.pack()

    frame = tk.Frame(root, bd=2)
    frame.place(relx=0.5, rely=0.1, relwidth=0.75, relheight=0.3, anchor='n')

    label1 = tk.Label(frame, text="SN")
    label1.place(relx=0, rely=0, relwidth=0.1, relheight=0.1)

    entry = tk.Entry(frame, font=20)
    entry.place(relx=0.1, rely=0, relwidth=0.25, relheight=0.2)

    button2 = tk.Button(frame, text="Exit", font=20, command=root.destroy)
    button2.place(relx=0.7, rely=0, relwidth=0.15, relheight=0.2)

    lower_frame = tk.Frame(root, bg='#80c1ff', bd=10)
    lower_frame.place(relx=0.5, rely=0.25, relwidth=0.75, relheight=0.75, anchor='n')

    label2 = tk.Label(lower_frame, anchor='nw', justify='left')
    label2.place(relwidth=1, relheight=1)

    button1 = tk.Button(frame, text="Start", font=20, command=lambda: cmd_with_ui(tree))
    button1.place(relx=0.4, rely=0, relwidth=0.15, relheight=0.2)

    tree = ttk.Treeview(lower_frame, columns=['1', '2', '3'], show='headings')
    tree.column('1', width=100, anchor='center')
    tree.column('2', width=100, anchor='center')
    tree.column('3', width=100, anchor='center')
    tree.heading('1', text='ITEM')
    tree.heading('2', text='RESULT')
    tree.heading('3', text='PASS/FAIL')
    tree.place(relwidth=1, relheight=1)
    root.mainloop()


if __name__ == "__main__":
    my_ui()
